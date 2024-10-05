### Apply the YAML files
- For Namespace A
```
kubectl apply -f pod-ns-a.yaml
```
- For Namespace B
```
kubectl apply -f pod-ns-b.yaml
```
- For Namespace C
```
kubectl apply -f pod-ns-c.yaml
```
### Apply Network Policy
- Apply the network policy for Namespace A
```
kubectl apply -f network-policy.yaml
```
### List the Network Policy
```
kubectl get netpol
```
### Describe Network Policy
```
kubectl describe netpol <np name>
```
### Delete Network Policy
```
kubectl delete netpol <np name>
```
