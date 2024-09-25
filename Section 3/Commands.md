# ClusterIP

### Creating ClusterIP using Ad-hoc commands

- create a pod
```
kubectl run apache --image=httpd -n dev--labels app=webapp 
```
- Using expose command. Even though if you don't specify the --type it will take clusterip by default. 
```  
kubectl expose pod apache --port=80 --name=httpd-clusterip -n dev --type=ClusterIp
```

### Creating ClusterIP using YAML files

- Apply pod.yaml

```
kubectl apply -f clusterip.yaml
```

### Access the application

- First exec into the pod

```
kubectl exec -it webserver -- sh
```
- Then run the curl command

```
curl <svc ip>
```
- You can also run curl command within the cluster
```
curl <svc IP>
```

# NodePort

### Creating NodePort using Ad-hoc commands

- create a pod
```
kubectl run apache --image=httpd -n dev--labels app=webapp 
```
- Using expose command.
```  
kubectl expose pod apache --port=80 --name=httpd-clusterip -n dev --type=NodePort
```

### Creating NodePort using YAML files

- Apply pod.yaml

```
kubectl apply -f nodeport.yaml
```

### Access the application

- For accessing the application you can copy the public ip of your worker node and run it in the browser with the port.
