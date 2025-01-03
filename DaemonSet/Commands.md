### Creates DeamonSet
```
kubectl apply -f deamonset.yaml
```
### List DeamonSet
```
kubectl get ds
```
### Describe DeamonSet
```
kubectl describe ds <deamonset name>
```

### Change image
```
kubectl set image ds/nginx nginx=nginx:1.25.5
```

### Check currently which image is running in the deployment
```
kubectl describe ds nginx-ds |grep image
```

### Add worker node

- You need to add one more worker node in your cluster to check whether the deamonset will launch another copy of a pod in that newly added worker node. To add the worker node you can run the worker node script which we have seen in the cluster setup.
- Once the worker node is added then you can see the pod will be launched in the newly created node as well.

### Delete deamonset
```
kubectl delete ds <ds name>
