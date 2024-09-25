### Creating Replicaset

- Using YAML file
```
kubectl apply -f replicaset.yaml
```

- Using Ad-hoc Commands
```
kubectl create rs nginx-replicaset --image=nginx:latest --replicas=3 --port=80
```
- exposing the replicaset
```
kubectl expose rs nginx-replicaset --port=80 --target-port=80 --name=nginx-service --type=ClusterIP
```
### List and inspect the replicaset

- To list the replicaset

```
kubectl get rs <rs name>
```

- To inspect replicaset

```
kubectl describe rs <rs name>
```

### Update number of replicas

- You can change the number of replicas in the yaml file and re-apply the yaml file
- Or you can use ad-hoc command to update the number of replicas

```
kubectl scale --replicas=5 replicaset/<rs name>
```

### Update image of replicas

- You can update the image of replicas in the yaml file and re-apply the yaml file
- Or you can use ad-hoc command to update the image of replicas

```
kubectl set image replicaset/<rs name> nginx=nginx:1.21.0
```

### Delete Replicaset

```
kubectl delete rs <rs name>
```
