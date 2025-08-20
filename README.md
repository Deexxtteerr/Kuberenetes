# Kubernetes Learning Project

Complete Kubernetes setup with nginx deployment, services, and Kind cluster configuration.

## 📁 Project Structure

├── nginx/                          # Nginx Kubernetes configurations
│   ├── deployment.yml              # Nginx deployment
│   ├── service.yml                 # Nginx service  
│   ├── pod.yml                     # Standalone nginx pod
│   ├── replicasets.yml             # ReplicaSet configuration
│   ├── deamonsets.yml              # DaemonSet configuration
│   ├── job.yml                     # Job configuration
│   ├── cron-job.yml                # CronJob configuration
│   ├── namespace.yml               # Namespace configuration
│   ├── persistentVolume.yml        # PersistentVolume configuration
│   └── persistentVolumeclaim.yml   # PersistentVolumeClaim configuration
├── kind-cluster/                   # Kind cluster configuration
│   └── config.yml                  # Multi-node cluster setup
└── scripts/                        # Installation scripts
   └── install.sh                  # Docker, Kind, kubectl installer


