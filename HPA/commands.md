### Deploy the Application
- To test the working of HPA you need to run the sample application. This YAML file you can get from the kubernetes official website.

```
kubectl apply -f php-pod.yaml
```
### Deploy HPA
- After deploying the applicaation you need to create the HPA
```
kubectl apply -f HPA.yaml
```
- You can also run below Ad-Hoc command to create HPA
```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=5
```
### Load Testing

- Once both sample application and hpa is deployed the you can run the below command to generate the load.
```
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"
```
### List the HPA
- After runing the load generator command. you can list the HPA wit watch command to check live status of cpu utilization getting increased
```
watch kubectl get HPA php-apache
```
### Describe HPA
- You can also describe the HPA
```
kubectl describe hpa php-apache
```
### Delete HPA
```
kubectl delete hpa php-apache
```
