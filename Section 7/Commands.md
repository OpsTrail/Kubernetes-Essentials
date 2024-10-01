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

### Annotation 
```
kubectl annotate deployment/nginx-deployment kubernetes.io/change-cause="image updated to 1.16.1"
```

### Check rollout history

```
kubectl rollout history ds/<ds-name>
```

### Check the rollout status

```
kubectl rollout status ds/nginx
```

### Rollback

- Rollback to the previous version

```
kubectl rollout undo ds/nginx-ds --to-revision=2
```

### Add worker node

- You need to add one more worker node in your cluster To check whether the deamonset will launch another copy of a pod in that newly added worker node. To add the worker node you can run the worker node script which we have seen in the cluster setup.
