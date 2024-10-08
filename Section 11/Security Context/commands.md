
### Verify the id which we have given
```
kubectl exec -it secure-pod -- id
```
### Check for privileged access
- Since we have set privilege escalation as false the below command won't work
```
kubectl exec -it secure-pod -- su root
```
### Checking for CHOWN capabilities
- Create a file. This command wont work because we have given **readOnlyRootFilesystem** as true even after we mentioned the capabilities it will not allow to create the file in the root file system.
```
kubectl exec -it secure-pod -- touch /tmp/testfile
```
