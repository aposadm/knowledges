# Docker Registry TLS Certificate Fix

## Problem

When connecting to a private Docker registry (e.g., Harbor) using a self-signed or internally signed certificate, Docker will refuse the connection with:

```
tls: failed to verify certificate: x509: certificate signed by unknown authority
```

This is a classic self-signed/private CA certificate trust issue with Docker.

---

## Fix: Step-by-Step

### 1. Get the Certificate from the Registry

```bash
openssl s_client -showcerts -connect reg.apos.com:443 </dev/null 2>/dev/null \
  | sed -n '/-----BEGIN CERTIFICATE-----/,/-----END CERTIFICATE-----/p' > ca.crt
```

> **Note:** Do not include `https://` in the `-connect` argument — use `host:port` format only.

### 2. Place It in Docker's Cert Store

```bash
mkdir -p /etc/docker/certs.d/reg.apos.com
cp ca.crt /etc/docker/certs.d/reg.apos.com/ca.crt
```

### 3. Restart Docker

```bash
systemctl restart docker
```

### 4. Retry the Login

```bash
echo "$HARBOR_PASSWORD" | docker login reg.apos.com -u "$HARBOR_USERNAME" --password-stdin
```

---

## GitLab CI/CD Runner Fix

The certificate must be trusted on the **runner host**, not just locally. Choose one of the options below.

### Option A — Add Cert to the Runner's System Trust Store (Recommended)

```bash
cp ca.crt /usr/local/share/ca-certificates/reg.apos.com.crt
update-ca-certificates
systemctl restart docker
```

### Option B — Disable TLS Verification in GitLab Runner Config

Edit `/etc/gitlab-runner/config.toml`:

```toml
[[runners]]
  [runners.docker]
    tls_verify = false   # ⚠️ Quick fix only — not recommended for production
```

### Option C — Bake the Cert into Your CI Job

Use this if you cannot modify the runner host directly.

In `.gitlab-ci.yml`:

```yaml
before_script:
  - mkdir -p /etc/docker/certs.d/reg.apos.com
  - echo "$HARBOR_CA_CERT" | base64 -d > /etc/docker/certs.d/reg.apos.com/ca.crt
```

Then store the base64-encoded certificate as a CI/CD variable named `HARBOR_CA_CERT` in GitLab:

```bash
# Generate the base64 value to paste into GitLab CI/CD variables
base64 -w 0 ca.crt
```

---

## Common Mistakes

### Wrong: Using `https://` in image tag

```
ERROR: invalid tag "https://reg.apos.com/project/image:abc123": invalid reference format
```

Docker image references must **never** include a protocol prefix. Fix your variable:

```bash
# ❌ Wrong
IMAGE_NAME=https://reg.apos.com/project/image

# ✅ Correct
IMAGE_NAME=reg.apos.com/project/image
```

If `IMAGE_NAME` is derived from `HARBOR_URL`, strip the protocol dynamically:

```yaml
before_script:
  - REGISTRY=$(echo "$HARBOR_URL" | sed 's|https://||')
  - IMAGE_NAME="$REGISTRY/project/image"
```

### Wrong: Using `https://` in `openssl s_client`

```bash
# ❌ Wrong
openssl s_client -connect https://reg.apos.com

# ✅ Correct
openssl s_client -connect reg.apos.com:443
```

---

## Root Cause

Docker strictly verifies TLS certificates. If Harbor uses a self-signed certificate or an internal CA, the Docker daemon on the runner will not trust it unless the CA certificate is explicitly added to the trust store.

