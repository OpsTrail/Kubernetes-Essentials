# Creating a User, Cert file and key

### Generate a Private Key for the John user
```
openssl genrsa -out john.key 2048
```
###  Creating a certificate signing request (CSR) for John
```
openssl req -new -key john.key -out john.csr -subj "/CN=john/O=dev-team"
```
### Signing the CSR using the Kubernetes CA cert
```
openssl x509 -req -in john.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out john.crt -days 365
```
### Copy Cert file, key and conf file
- Once you generate the cert file and key for John user. Copy those files in a particular location.
```
cp john.crt /etc/kubernetes/pki/
```
```
cp john.key /etc/kubernetes/pki/
```
```
cp john.conf /etc/kubernetes/
```
### Export Kubeconfig
```
export KUBECONFIG=/etc/kubernetes/john.conf
```

# Creating Role and Role Bindings

### Create Role
- Apply YAML file
```
kubectl apply -f role.yaml
```
### Create RoleBindings
- Apply YAML file
```
kubectl apply -f rolebinding.yaml
```
### List roles and Role Binding
- Role
```
kubectl get role -n development
```
- Role Binding
```
kubectl get rolebindings -n development
```
