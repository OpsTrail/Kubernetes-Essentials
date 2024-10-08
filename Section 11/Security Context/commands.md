
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
- Create a file. This command wont work because we have given **readonlyrootfilesystem** as true 
```
kubectl exec -it secure-pod -- touch /tmp/testfile
```
