### Install Metrics server
- You can get latest vertion from this site: https://github.com/kubernetes-sigs/metrics-server/releases
```
wget https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.7.1/components.yaml
```
- After downloading the file add this flag in the deployment section
```
- --kubelet-insecure-tls
```
- Then apply the components.yaml
```
kubectl apply -f components.yaml
```

### Check Matrics
- For Nodes
```
kubectl top nodes
```
- For Pods
```
kubectl top pods
```

### Test CPU limit

- Apply YAML file
```
kubectl apply -f cpu-demo.yaml
```
- Exec into pod to to increase CPU limit
```
kubectl exec -it resource-demo -- sh -c "while :; do :; done"
```

### Test Memory Limit(OOM)
- Apply YAML file
```
kubectl apply -f memory-demo.yaml
```
- List the pod and see the OOM status
```
kubectl get pods
```
