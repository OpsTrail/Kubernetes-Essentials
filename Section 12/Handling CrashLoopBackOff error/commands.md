# Scenario 1: Wrong command Passed to the container

- You need to first apply crashloop-demo.yaml
```
kubectl apply -f crashloop-demo.yaml
```
- Since in the YAML file we are exiting with error code 1. Which will restart the pod repeatedly and lead to crashloopbackoff error.

# Scenario 2: Port conflict

- You need to first apply port-conflict.yaml
```
kubectl apply -f port-conflict.yaml
```
- In this yaml file we are trying to allocate port 80 to both the containers which leads to port conflict and the pod will go to crashloopbackoff error.


# Debugging for both scenarios

### Check Logs

- To debug the issue you can check the logs of the pod
```
kubectl logs <pod name> -n <namespace name>
```
- If you have multiple containers
```
kubectl logs <pod name> -c <container name> -n <namespace name>
```
- You can also check the logs of the previous pods
```
kubectl logs <pod name> -n <namespace> --previous
```
- To check the logs of all the contaners
```
kubectl logs <pod name> -n <namespace> --all-containers
```
### Describe Pod
- You can also do the describe to check the events related to that pods
```
kubectl describe pod <pod name>
```
### Check events
- You can also check the events
```
kubectl get events
```
