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

### PersistentVolume
- Storage: 1Gi
- Access Mode: ReadWriteOnce
- Storage Type: hostPath (for demonstration)

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

- This setup uses hostPath for demonstration purposes. In production, use appropriate storage solutions like AWS EBS, Azure Disk, or GCP Persistent Disk.
- The PVC is configured with ReadWriteOnce access mode.
- The service is exposed as NodePort for external access.

## Contributing

Feel free to submit issues and enhancement requests! 