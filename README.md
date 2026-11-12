# Kubernetes Learning Notes

This file is my Kubernetes study notebook. Here I keep the important concepts, commands, and practical examples I learn while working with Kubernetes.

## 1. Kubernetes basics

Kubernetes (K8s) is an open-source container orchestration platform used to deploy, scale, and manage containerized applications.

### Main purpose
- Run containers reliably
- Auto-healing failed containers
- Scale applications up/down based on load
- Balance traffic using Services
- Manage deployment updates and rollbacks

### Important Kubernetes objects
- Pod: the smallest deployable unit
- Deployment: manages ReplicaSets and app rollout
- Service: provides stable network access to Pods
- ReplicaSet: ensures a desired number of Pod replicas
- Namespace: logical environment separation
- ConfigMap: configuration data
- Secret: sensitive data like passwords and tokens
- Volume: persistent storage for Pods
- Ingress: HTTP/HTTPS routing
- StatefulSet: manages stateful apps
- DaemonSet: runs one Pod on each node
- Job/CronJob: batch and scheduled jobs

## 2. Typical architecture

- Control Plane: API Server, Scheduler, Controller Manager, etcd
- Worker Node: kubelet, kube-proxy, container runtime

### Common terms
- Cluster: group of nodes
- Node: machine where containers run
- Pod: group of one or more containers
- Label: key/value metadata used for selection
- Selector: finds Pods by label
- Namespace: logical boundary inside cluster

## 3. Setup and verification commands

### Check Kubernetes version
```bash
kubectl version --client
kubectl version
```

### Cluster information
```bash
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
```

### View all resources
```bash
kubectl get all
kubectl get all -A
kubectl get ns
```

### Resource details
```bash
kubectl describe node <node-name>
kubectl describe pod <pod-name>
kubectl describe deployment <deployment-name>
kubectl describe svc <service-name>
```

## 4. Working with Pods

### Create Pod from YAML
```bash
kubectl apply -f pod/pod.yml
```

### Check Pods
```bash
kubectl get pods
kubectl get pods -o wide
```

### View Pod logs
```bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>
```

### Execute inside a running container
```bash
kubectl exec -it <pod-name> -- /bin/bash
kubectl exec -it <pod-name> -- sh
```

### Delete Pod
```bash
kubectl delete pod <pod-name>
kubectl delete -f pod/pod.yml
```

### Port forward for local access
```bash
kubectl port-forward pod/<pod-name> 8080:80
kubectl port-forward svc/<service-name> 8080:80
```

## 5. Deployment commands

### Apply deployment
```bash
kubectl apply -f deployment/deployment.yml
```

### Check Deployments
```bash
kubectl get deployment
kubectl get deploy
kubectl get rs
```

### See rollout status
```bash
kubectl rollout status deployment/<deployment-name>
```

### Scale replicas
```bash
kubectl scale deployment/<deployment-name> --replicas=5
```

### Update image
```bash
kubectl set image deployment/<deployment-name> nginx=nginx:1.26
```

### Rollback deployment
```bash
kubectl rollout undo deployment/<deployment-name>
```

### History of rollout
```bash
kubectl rollout history deployment/<deployment-name>
```

## 6. Service commands

### Apply service
```bash
kubectl apply -f service/service.yml
```

### Get services
```bash
kubectl get svc
kubectl get service
```

### Check service details
```bash
kubectl describe svc <service-name>
kubectl get endpoints
```

### Delete service
```bash
kubectl delete svc <service-name>
```

## 7. YAML basics for Kubernetes

A Kubernetes YAML file usually includes:
- apiVersion
- kind
- metadata
- spec

Example structure:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

### Important YAML concepts
- metadata.name: resource name
- metadata.labels: labels used for selecting resources
- spec.selector: selects matching Pods
- spec.replicas: number of instances
- spec.template: Pod template for Deployment
- ports.containerPort: port inside the container
- ports.targetPort: port in backend Pod
- ports.port: port exposed by the Service

## 8. Commands I should remember

### Show all resource types
```bash
kubectl api-resources
```

### Get YAML of an object
```bash
kubectl get pod <pod-name> -o yaml
kubectl get deploy <deployment-name> -o yaml
```

### Edit resource directly
```bash
kubectl edit deployment <deployment-name>
```

### Delete all resources in a namespace
```bash
kubectl delete all --all -n <namespace>
```

### Run a temporary Pod for testing
```bash
kubectl run test-pod --image=busybox:1.36 --rm -it --restart=Never -- sh
```

### Check events
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Resource usage
```bash
kubectl top node
kubectl top pod
```

## 9. Useful namespaces commands

```bash
kubectl get ns
kubectl create namespace dev
kubectl config set-context --current --namespace=dev
kubectl get pods -n default
```

## 10. Common troubleshooting commands

```bash
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get events -A
kubectl get svc -A
kubectl get endpoints -A
```

### When Pod is not starting
- Check image name
- Check container port
- Check ConfigMap / Secret
- Check resource limits
- Check events
- Check readiness/liveness probes

## 11. Deployment workflow example

1. Create Pod / Deployment YAML
2. Run `kubectl apply -f ...`
3. Check status with `kubectl get pods`
4. Use `kubectl describe` if something fails
5. View logs with `kubectl logs`
6. Expose app via Service
7. Test with `kubectl port-forward` or cluster IP
8. Scale / upgrade / rollback as needed

## 12. Examples in this repository

This folder includes sample Kubernetes files:
- `pod/pod.yml` — simple Pod example
- `deployment/deployment.yml` — Deployment with 3 replicas
- `service/service.yml` — ClusterIP Service example

## 13. Quick cheat sheet

```bash
kubectl apply -f <file.yaml>
kubectl get pods
kubectl get svc
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- sh
kubectl delete -f <file.yaml>
kubectl rollout status deployment/<deployment-name>
kubectl scale deployment/<deployment-name> --replicas=3
kubectl rollout undo deployment/<deployment-name>
```

## 14. Notes for my learning progress

- Kubernetes is all about declarative configuration
- YAML defines desired state
- `kubectl apply` is the common command for creating/updating resources
- Labels and selectors are essential for Services and Deployments
- Pod health must be monitored using logs, describe, and probes
- Service is needed to access Pods reliably even if Pod IPs change
- Deployment is preferred over direct Pod management for apps

## 15. Future learning topics

- Ingress and NGINX Ingress Controller
- Helm charts
- ConfigMap and Secret management
- Persistent Volumes and PVC
- StatefulSet for databases
- Horizontal Pod Autoscaler (HPA)
- Kubernetes networking and DNS
- RBAC and security
- Monitoring and logging with Prometheus and Grafana
- CI/CD with Kubernetes

## 16. Summary

Kubernetes learning is mostly about understanding:
- objects and resources
- YAML definition
- desired state management
- networking and service exposure
- scaling and rolling updates
- troubleshooting and observability

This README will be updated as I continue learning.
