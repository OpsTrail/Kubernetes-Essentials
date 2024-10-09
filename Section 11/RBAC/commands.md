# Creating a User, Cert file and key

### Generate a Private Key for the John user
- The purpose of creating a private key and crt is to authorize and validate the John user.
```
openssl genrsa -out john.key 2048
```
###  Creating a certificate signing request (CSR) for John
- The csr will generate the certificate for john user that will be signed and validated by the trusted entities.
```
openssl req -new -key john.key -out john.csr -subj "/CN=john/O=dev-team"
```
### Signing the CSR using the Kubernetes CA cert
- Now that we have generated the CSR, Kubernetes will use the trusted entities which are ca.crt(Certificate authority certificate) and ca.key(Certificate authority Private Key) to add the digital signature and validate the John user.
- So whenever John makes any request to the API server like get pods. its certificate which the john.crt will be used to get the identity information of the user the private which is the john.key will be used to authenticate and encrypt the user request.
```
openssl x509 -req -in john.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out john.crt -days 365
```
### Copy Cert file, key and conf file
- Once you generate the cert file and key for John user. Copy those files at **/etc/kubernetes/pki/** although you can place these files wherever you want but it's a good practice to store all the cert files and keys in a common place.
```
cp john.crt /etc/kubernetes/pki/
```
```
cp john.key /etc/kubernetes/pki/
```
```
cp john.conf /etc/kubernetes/
```
### Check which user you are
```
kubectl auth whoami
```
### Check if you can access particular resource
```
kubectl auth can-i create pod
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
