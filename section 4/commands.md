# ReplicaSet

### Creating Replicaset

- Using YAML file
```
kubectl apply -f replicaset.yaml
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

### Delete Replicaset

```
kubectl delete rs <rs name>
```

# Deployments

### Creating Deployments

- Using Ad-hoc commands
```
kubectl create deployment httpd-deploy --image=httpd
```
- Using YAML file
```
kubectl apply -f deployment.yaml
```
- List the deployments
```
kubectl get deploy <deployment name>
```
- Inspecting deployments
```
kubectl describe deployment <deployment name>
```
- Setting New image for deployments
```
kubectl set image deployment/my-nginx nginx=nginx:1.21
```
- Exposing Deployment to svc
```
kubectl expose deployment my-nginx --type=LoadBalancer --port=80
```
- Scalling the deployments
```
kubectl scale deployment my-nginx --replicas=5
```
# Rolling Updates and Rollback

# Deployment Strategy: Recreate

- First Apply the YAML file to create a deployment
```
kubectl apply -f deploy.yaml
```
- List the deployment
```
kubectl get deploy
```
- Set new immage for deployment
```
kubectl set image deployment/my-app-deployment my-app-container=nginx:latest --record
```
- Check the rollout status
```
kubectl rollout status deployment/my-app-deployment
```
# Deployment Strategy: Blue/Green Deployment