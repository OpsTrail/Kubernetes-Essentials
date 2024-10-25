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
- You can also run the curl command within the cluster
```
curl <svc IP>
```

# NodePort

### Creating NodePort using Ad-hoc commands

- create a pod
```
kubectl run apache --image=httpd -n dev --labels app=webapp 
```
- Using expose command.
```  
kubectl expose pod apache --port=80 --name=httpd-nodeport -n dev --type=NodePort
```

### Creating NodePort using YAML files

- Apply nodeport.yaml

```
kubectl apply -f nodeport.yaml
```

### Access the application

To access the application, you can copy the public IP of your worker node and run it in a browser with the port.

# LoadBalncer

### Creating LoadBalancer using Ad-hoc commands

- create a pod
```
kubectl run apache --image=httpd -n dev --labels app=webapp 
```
- Using expose command.
```  
kubectl expose pod apache --port=80 --name=httpd-nodeport -n dev --type=LoadBalancer
```

### Creating NodePort using YAML files

- Apply loadbalancer.yaml

```
kubectl apply -f loadbalncer.yaml
```

### Access the application

- For accessing the application you can copy the load balancer dns and run it in the browser.
