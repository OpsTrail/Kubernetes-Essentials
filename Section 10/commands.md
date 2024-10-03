### Create ConfigMap
- Using Ad-Hoc Command
```
kubectl create configmap config-map --from-literal=APP_ENV=production --from-literal=APP_LOG_LEVEL=info
```
- Using YAML file
```
kubectl apply -f configmap.yaml
```
### Create Secret
- Using Ad-hoc command
```
kubectl create secret generic secretdb --from-literal=DB_USER=admin --from-literal=DB_PASSWORD=secretpassword
```
- Using YAML file
```
kubectl apply -f secrets.yaml
```
### Using ConfigMap and Secrets in pod
```
kubectl apply -f pod-config-secret.yaml
```
