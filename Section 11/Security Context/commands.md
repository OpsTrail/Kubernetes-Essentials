

```
kubectl exec -it secure-pod -- id
```

```
kubectl exec -it secure-pod -- su root
```
```
kubectl exec -it secure-pod -- touch /tmp/testfile
```
```
kubectl exec -it secure-pod -- chown 2000:2000 /tmp/testfile
```
```
kubectl exec -it secure-pod -- ls -l /tmp/testfile
```
