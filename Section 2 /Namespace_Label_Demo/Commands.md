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
- Here you can get idea that this namespace is for preprod environment and for nginx web application.

```
kubectl create ns preprod --labels env=preprod,app=nginx
```

### Creating Pods with namespace and labels

- Creating a pod with Ad-Hoc command
```
kubectl run nginx --image=nginx -n preprod --labels env=preprod,app=webapp
```
```
kubectl apply -f pod.yaml
```
