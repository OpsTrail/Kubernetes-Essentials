# Nginx-Ingress Controller
- To install the Nginx Ingress Controller to your cluster, you’ll first need to add its repository to Helm by running
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
- After adding the repo do an update
```
helm repo update
```
- Finally, run the following command to install the Nginx ingress
```
helm install nginx-ingress ingress-nginx/ingress-nginx --version 1.4.0 --set controller.publishService.enabled=true 
```
- You can watch the Load Balancer become available by running
```
kubectl --namespace default get services -o wide -w nginx-ingress-ingress-nginx-controller
```
# Create Ingress resource and Deployment
### Ingress resource
- Apply ingress file
```
kubectl apply -f ingress.yaml
```
### Create Deployment
- Apply first-deployment.yaml
```
kubectl apply -f first-deployment.yaml
```
- Apply Second-deployment.yaml
```
kubectl apply -f second-deployment.yaml
```
### Add External IP to Windows host file
- Host file will act as local DNS resolver. If you have your domain either in Rouute53 or any other DNS provider you can add the loadBalncer DNS in the records. Other add it in local Host File

**In windows**
```
C:\Windows\System32\drivers\etc\hosts
```
