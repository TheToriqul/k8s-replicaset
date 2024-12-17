# Kubernetes ReplicaSet Command Reference Guide

### Project Content Table
- [Core Operations](#core-operations)
- [Pod Management](#pod-management)
- [Scaling Operations](#scaling-operations)
- [Troubleshooting](#troubleshooting)
- [Cleanup](#cleanup)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Comprehensive command reference for managing NGINX ReplicaSets in Kubernetes  
> **Note**: Review commands carefully before execution in your environment.

## Core Operations

### Deploy ReplicaSet
```bash
# Apply the ReplicaSet configuration
kubectl apply -f replicaset.yaml

# Verify ReplicaSet creation
kubectl get replicaset nginx-replicaset

# Check ReplicaSet details
kubectl describe replicaset nginx-replicaset
```

### View Pod Status
```bash
# List all pods managed by ReplicaSet
kubectl get pods --selector=app=nginx

# Get detailed pod information
kubectl describe pods --selector=app=nginx

# Watch pod status in real-time
kubectl get pods --selector=app=nginx --watch
```

## Pod Management

### Test Self-Healing
```bash
# Delete a specific pod (ReplicaSet will recreate it)
kubectl delete pod <pod-name>

# Delete all pods (ReplicaSet will recreate all)
kubectl delete pods --selector=app=nginx

# Verify pod regeneration
kubectl get pods --selector=app=nginx
```

### Pod Inspection
```bash
# View pod logs
kubectl logs <pod-name>

# Get pod details in YAML format
kubectl get pod <pod-name> -o yaml

# Check pod events
kubectl describe pod <pod-name> | grep -A 10 Events
```

## Scaling Operations

### Manual Scaling
```bash
# Scale ReplicaSet to 5 replicas
kubectl scale replicaset nginx-replicaset --replicas=5

# Verify scaling operation
kubectl get replicaset nginx-replicaset

# Monitor pod creation
kubectl get pods --selector=app=nginx --watch
```

### Update Configuration
```bash
# Edit ReplicaSet configuration
kubectl edit replicaset nginx-replicaset

# Apply updated configuration file
kubectl apply -f replicaset.yaml

# Check update status
kubectl describe replicaset nginx-replicaset
```

## Troubleshooting

### Health Checks
```bash
# Check ReplicaSet events
kubectl describe replicaset nginx-replicaset | grep -A 10 Events

# View pod status
kubectl get pods --selector=app=nginx -o wide

# Check pod readiness
kubectl get pods --selector=app=nginx -o custom-columns=NAME:.metadata.name,READY:.status.conditions[?(@.type=='Ready')].status
```

### Debugging
```bash
# Execute command in pod
kubectl exec -it <pod-name> -- /bin/bash

# Check NGINX configuration
kubectl exec -it <pod-name> -- nginx -t

# View NGINX logs
kubectl exec -it <pod-name> -- tail -f /var/log/nginx/access.log
```

## Cleanup

### Resource Removal
```bash
# Delete ReplicaSet and pods
kubectl delete replicaset nginx-replicaset

# Verify removal
kubectl get replicaset
kubectl get pods --selector=app=nginx

# Remove configuration file
rm replicaset.yaml
```

## Learning Notes

1. ReplicaSets ensure desired number of pod replicas are always running
2. Pod labels and selectors are crucial for ReplicaSet management
3. Self-healing occurs automatically when pods are deleted
4. Scaling can be performed without modifying the YAML file
5. Pod regeneration is nearly instantaneous

---

> 💡 **Best Practice**: Always use labels to organize and identify resources

> ⚠️ **Warning**: Deleting a ReplicaSet will delete all managed pods

> 📝 **Note**: ReplicaSet names must be unique within a namespace