
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
- Create a file.
```
kubectl exec -it secure-pod -- touch /tmp/testfile
```
- Change the ownership
```
kubectl exec -it secure-pod -- chown 2000:2000 /tmp/testfile
```
- Check the ownership
```
kubectl exec -it secure-pod -- ls -l /tmp/testfile
```
