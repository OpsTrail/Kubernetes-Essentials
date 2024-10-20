# Add Helm Repository
- Add stable helm repository
```
helm repo add stable https://charts.helm.sh/stable
```
- Add Prometheus and Grafana repository
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
# Installing Prometheus and Grafana

- Create a Prometheus namespace
```
kubectl create namespace prometheus
```
- Install the Prometheus stack.
```
helm install stable prometheus-community/kube-prometheus-stack --version 48.3.1 -n prometheus
```
- List The pod
```
kubectl get pods -n prometheus
```
- List the SVC
```
kubectl get svc -n prometheus
```
**Edit the SVC and change the type of the SVC to NodePort to access it from the browser**
- For Prometheus
```
kubectl edit svc stable-grafana -n prometheus
```
- For Grafana
```
kubectl edit svc stable-grafana -n prometheus
```
### Access the Grafana dashboard
- Get the Password for the grafana dashboard user name will be **admin**
```
kubectl get secret stable-grafana -o jsonpath="{.data.admin-password}" -n prometheus | base64 --decode ; echo
```
### Import Grafana Dashboard
- You can import the different dashboards from this link: https://grafana.com/grafana/dashboards/
- Get the dashboard ID and import it.
**For the demo, I am using:**
- 8171 - nodes metrics
- 741 - deployments metrics
