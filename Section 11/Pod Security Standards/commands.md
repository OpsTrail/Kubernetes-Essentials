### Create Namespace
```
kubectl create ns dev
```
```
kubectl create ns staging
```
```
kubectl create ns prod
```
# Add Enforce mode
- Assign Labels to the namespace
```
kubectl label ns dev pod-security.kubernetes.io/enforce=baseline
```
```
kubectl label ns prod pod-security.kubernetes.io/enforce=restricted
```
```
kubectl label ns staging pod-security.kubernetes.io/enforce=privileged
```
### 1. In this scenario we have given enforce mode in the namespace label
**Execute the below commands none of them should throw an error.**
```
kubectl apply -f privileged-pod.yaml -n staging
```
```
kubectl apply -f baseline-pod.yaml -n dev
```
```
kubectl apply -f restricted-pod.yaml -n prod
```
 
**Execute the below command to create pod in different namespace.**
- This command should throw an error because we are trying to execute privileged access pod in restricted mode namespace in this case pod will not create.
```
kubectl apply -f privileged-pod.yaml -n prod  
```
- This command should throw an error because we are trying to execute privileged access pod in baseline level namespace which should not have privileged access in this case pod will not create.
```
kubectl apply -f privileged-pod.yaml -n dev
```
- This command should throw an error because we are trying to execute baseline pod with capabilities in restricted level namespace which does not allow any capabilities to be added in this case pod will not create.
```
kubectl apply -f baseline-pod.yaml -n prod   
```
