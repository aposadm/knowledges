# longhorn-values.yaml

# Longhorn Manager
longhornManager:
  priorityClass: system-cluster-critical
  tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Exists"
      effect: "NoSchedule"

# Longhorn Driver
longhornDriver:
  priorityClass: system-cluster-critical
  tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Exists"
      effect: "NoSchedule"

# UI Configuration
longhornUI:
  replicas: 2
  priorityClass: system-cluster-critical

# Default Settings
defaultSettings:
  # Backup target (S3, NFS, etc.)
  backupTarget: "s3://longhorn-backups@us-east-1/"
  backupTargetCredentialSecret: longhorn-backup-secret
  
  # Default replica count
  defaultReplicaCount: 3
  
  # Storage over-provisioning
  storageOverProvisioningPercentage: 200
  storageMinimalAvailablePercentage: 10
  
  # Replica soft anti-affinity
  replicaSoftAntiAffinity: true
  replicaAutoBalance: best-effort
  
  # Default data locality
  defaultDataLocality: best-effort
  
  # Node drain policy
  nodeDrainPolicy: block-if-contains-last-replica
  
  # Automatic salvage
  autoSalvage: true
  
  # Guaranteed engine manager CPU
  guaranteedEngineManagerCPU: 12
  guaranteedReplicaManagerCPU: 12
  
  # Snapshot data integrity
  snapshotDataIntegrity: fast-check
  
  # Recurring job settings
  recurringSuccessfulJobsHistoryLimit: 1
  recurringFailedJobsHistoryLimit: 1

# Default Storage Class
persistence:
  defaultClass: true
  defaultClassReplicaCount: 3
  defaultFsType: ext4
  defaultMkfsParams: ""
  defaultDataLocality: best-effort
  reclaimPolicy: Delete
  migratable: false

# CSI Configuration
csi:
  attacherReplicaCount: 3
  provisionerReplicaCount: 3
  resizerReplicaCount: 3
  snapshotterReplicaCount: 3

# Ingress for UI
ingress:
  enabled: true
  ingressClassName: nginx
  host: longhorn.example.com
  tls: true
  tlsSecret: longhorn-tls
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/auth-type: basic
    nginx.ingress.kubernetes.io/auth-secret: longhorn-basic-auth

# Service Monitor for Prometheus
serviceMonitor:
  enabled: true
