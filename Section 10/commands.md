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
- Apply YAML file
```
kubectl apply -f pod-config-secret.yaml
```
- Exec into the POD
```
kubectl exec -it config-secret-demo -- /bin/sh
```
- And check the environment variables inside the pod
```
printenv
```
```
cat /etc/config/APP_ENV
cat /etc/config/APP_LOG_LEVEL
```
```
cat /etc/secret/DB_USER
cat /etc/secret/DB_PASSWORD
```
