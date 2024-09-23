### Namespace Creation

- using Ad-Hoc command

```
kubetcl create ns <namespace name>
```

- using YAML file

```
kubectl apply -f namespace.yaml
```

### Label Creation

- For creating labels there is no such YAML you need to create you can simply add labels either through Ad-Hoc commands or under the metadata section. Labels can be created for any objects that are present in the cluster and for nodes as well.

**Creating Labels for namespace, Pod, Nodes**

- Create a Namespace
```
kubectl create ns prod
```
- Adding labels
```
kubectl label ns prod app=production
```
- Adding labels during namespace creation
- Here you can get the idea that this namespace is for the preprod environment and for the nginx web application.

```
kubectl create ns preprod --labels env=preprod,app=nginx
```

### Creating Pods with namespace and labels

- Creating a pod with an Ad-Hoc command
```
kubectl run nginx --image=nginx -n preprod --labels env=preprod,app=webapp
```

- Specifying label and namespace in the YAML file itself and applying
```
kubectl apply -f pod.yaml
```
- Adding labels to the running pod
```
kubectl label pod webserver tier=frontend -n dev
```

- Overwrite the existing labels
```
kubectl label pod webserver env=production -n dev --overwrite
```

### list down the labels

- Now if you want to see what labels are associated with the pod or a namespace

```
kubectl get pod webserver -n dev --show-labels
```
```
kubectl get ns dev --show-labels
```

### Removing any label

- Removing labels from namespace
```
kubectl label ns dev env-
```
- Removing Labels from Pod
```
kubectl label pod webserver env-
```
### Deleting resources based on the labels

- Deleting a pod based on the labels
```
kubectl delete pod -l app=nginx -n dev
```
