### Pod Creation

- By applying yaml files
```
kubectl apply -f pod.yaml
```

- Using Ad-Hoc commands
```
kubectl run apache --image=httpd
```

- Generating the yaml file using Ad-Hoc commands
```
kubectl run apache2 --image=httpd -o yaml --dry-run=client > apache.yaml
```

- Creating pod by specfing command during the pod creation
```
kubectl run busybox --image=busybox --command -- sh -c "while true; do echo Hello from BusyBox; sleep 2; done"
```

### Getting pod information

- Check how many pod are running
```
kubectl get pods
```

- Check how many pod are running and get its IP and node
```
kubectl get pods -o wide
```
### Inspect details and Events of the pod

- Get all the details regarding the pod and its events
```
kubectl describe pod <pod name>
```

### How to create multicontainer pod

- By applying YAML file only
```
kubectl apply -f multipod.yaml
```
**Note:** You can't create multicontainer pod using Ad-Hoc commands

### How to exec into the pod

- if single container
```
kubectl exec -it webserver -- sh
```

- if multiple container

```
kubectl exec -it multicontainer -c busybox -- sh
```

### How to check logs

```
kubectl logs busybox
```

### How to delete pod

- Using YAML file
```
Kubectl delete -f pod.yaml
```
- delete using Pod Name
```
kubectl delete pod <pod name>
```
- Delete All the Pods
```
kubectl delete pod --all
```
