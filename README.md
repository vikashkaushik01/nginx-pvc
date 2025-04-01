# Nginx Deployment with Persistent Volume

This repository contains Kubernetes manifests for deploying Nginx with a PersistentVolume (PV) and PersistentVolumeClaim (PVC). The setup allows you to serve custom web content that persists across pod restarts.

## Prerequisites

- Kubernetes cluster
- kubectl CLI tool
- Git

## Repository Structure

```
.
├── README.md
├── nginx-pv.yaml         # PersistentVolume and PersistentVolumeClaim definitions
├── nginx-deployment.yaml # Nginx deployment and service
└── index.html           # Custom webpage content
```

## Storage Configuration

### StorageClass Selection

The PVC is configured to use the `standard` StorageClass by default. To use a different StorageClass:

1. List available StorageClasses in your cluster:
```bash
kubectl get storageclass
```

2. Edit the `nginx-pv.yaml` file and update the `storageClassName` field:
```yaml
spec:
  storageClassName: your-storage-class-name  # Replace with your desired StorageClass
```

Common StorageClass options:
- `standard` - Default StorageClass in most clusters
- `managed-premium` - Premium storage (e.g., Azure Premium SSD)
- `gp2` - AWS EBS GP2 volumes
- `pd-standard` - Google Cloud Persistent Disk Standard
- `longhorn` - Longhorn storage
- `local-path` - Local storage (for development)

### Storage Requirements

- Storage Size: 1Gi
- Access Mode: ReadWriteOnce
- Storage Type: Dynamic provisioning based on StorageClass

## Deployment Steps

1. Clone the repository:
```bash
git clone <your-repo-url>
cd <repo-name>
```

2. Apply the PersistentVolume and PersistentVolumeClaim:
```bash
kubectl apply -f nginx-pv.yaml
```

3. Deploy Nginx with the persistent storage:
```bash
kubectl apply -f nginx-deployment.yaml
```

4. Get the pod name:
```bash
kubectl get pods
```

5. Copy the custom webpage content to the pod:
```bash
kubectl cp index.html <pod-name>:/usr/share/nginx/html/index.html
```

6. Get the NodePort to access the service:
```bash
kubectl get svc nginx-service
```

7. Access the webpage:
```
http://<node-ip>:<node-port>
```

## Configuration Details

### PersistentVolumeClaim
- Storage: 1Gi
- Access Mode: ReadWriteOnce
- Storage Class: standard (configurable)

### Nginx Deployment
- Image: nginx:latest
- Replicas: 1
- Port: 80
- Volume Mount: /usr/share/nginx/html

### Service
- Type: NodePort
- Port: 80
- Target Port: 80

## Cleanup

To remove the deployed resources:
```bash
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-pv.yaml
```

## Notes

- The PVC is configured with ReadWriteOnce access mode.
- The service is exposed as NodePort for external access.
- StorageClass selection depends on your cluster's available storage providers.
- For production environments, choose a StorageClass that matches your requirements for performance, availability, and cost.

## Contributing

Feel free to submit issues and enhancement requests! 