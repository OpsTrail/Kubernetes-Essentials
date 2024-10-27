# Nginx-Ingress Controller
- Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

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
helm install nginx-ingress ingress-nginx/ingress-nginx --set controller.publishService.enabled=true 
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
### In the case of GCP
- Host file will act as local DNS resolver. If you have your domain either in Rouute53 or any other DNS provider you can add the load balancer DNS in the records. Others add it in the local Host File

**In Windows**
- Open Notepad as administrator and open the host file in the Notepad at the given path
```
C:\Windows\System32\drivers\etc\hosts
```
- Once you open the Notepad add the external IP that you are getting on the GKE cluster. Make the entry as shown below and save the file.
- Now try to access the domain from your browser.
![image](https://github.com/user-attachments/assets/2ef736b4-9f63-4a13-8452-b765c17e174e)


