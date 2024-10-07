### Create Namespace
```
kubectl create ns dev
```
```
kubectl create ns staging
```
```
kubectl create ns prod
```
# Add Enforce mode
- Assign Labels to the namespace
```
kubectl label ns dev pod-security.kubernetes.io/enforce=baseline
```
```
kubectl label ns prod pod-security.kubernetes.io/enforce=restricted
```
```
kubectl label ns staging pod-security.kubernetes.io/enforce=privileged
```
### 1. In this scenario we have given enforce mode in the namespace label
**Execute the below commands none of them should throw an error.**
```
kubectl apply -f privileged-pod.yaml -n staging
```
```
kubectl apply -f baseline-pod.yaml -n dev
```
```
kubectl apply -f restricted-pod.yaml -n prod
```
 
**Execute the below command to create a pod in a different namespace.**
- This command should throw an error because we are trying to execute privileged access pod in restricted mode namespace in this case pod will not create.
```
kubectl apply -f privileged-pod.yaml -n prod  
```
- This command should throw an error because we are trying to execute privileged access pod in baseline level namespace which should not have privileged access in this case pod will not create.
```
kubectl apply -f privileged-pod.yaml -n dev
```
- This command should throw an error because we are trying to execute baseline pod with capabilities in restricted level namespace which does not allow any capabilities to be added in this case pod will not create.
```
kubectl apply -f baseline-pod.yaml -n prod   
```
**Make sure to delete pod by using below command in each namespace before going to next scenario.**
```
kubectl delete -f <yaml file name> -n <namespace name> --force
```
# Add Warn Mode

**In this scenario we have given a warn mode in the namespace label by executing the commands below.**

- Change label: pod-security.kubernetes.io/enforce=baseline as pod-security.kubernetes.io/warn=baseline

```
kubectl edit ns dev
```
- Change label: pod-security.kubernetes.io/enforce=restricted as pod-security.kubernetes.io/warn=restricted
```
kubectl edit ns prod
```
- Change label: pod-security.kubernetes.io/enforce=privileged as pod-security.kubernetes.io/warn=privileged 
```
kubectl edit ns staging
```
**Execute the below commands none of them should throw an error and warning.**
```
kubectl apply -f privileged-pod.yaml -n staging
```
```
kubectl apply -f baseline-pod.yaml -n dev
```
```
kubectl apply -f restricted-pod.yaml -n prod
```
**Execute the below command to create a pod in a different namespace.**

- In this case, the pod will be created by throwing a warning message of violation of the restricted level.
```
kubectl apply -f privileged-pod.yaml -n prod 
```
- In this case, the pod will be created by throwing a warning message of violation of baseline level.
```
kubectl apply -f privileged-pod.yaml -n dev 
```
- In this case, a pod will be created by throwing a warning message about a violation of the restricted level.
```
kubectl apply -f baseline-pod.yaml -n prod 
```
**Make sure to delete the pod by using the below command in each namespace before going to the next scenario.**
```
kubectl delete -f <yaml file name> -n <namespace name> --force
```
# Enabling Auditing and Audit Mode
### Changing to Audit Mode
- change label: pod-security.kubernetes.io/warn=baseline as pod-security.kubernetes.io/audit=baseline
```
kubectl edit ns dev 
```
- change label: pod-security.kubernetes.io/warn=restricted as pod-security.kubernetes.io/audit=restricted
```
kubectl edit ns prod
```
- change label: pod-security.kubernetes.io/warn=privileged as pod-security.kubernetes.io/audit=privileged 
```
kubectl edit ns staging 
```
### Enabling Auditing

- Create the given audit-policy.yaml which defines what we need to audit. In this YAML file, we are auditing request, request-response, and metadata for the namespace prod. You need to store the YAML file  at the location: **/etc/kubernetes/**

### Doing changes in API server

- Now to enable auditing we need to make some changes in the api-server configuration file. Before making any changes to the original kube-apiserver.yaml it is best practice to make a copy of the file and then make the changes.

- Go to /etc/kubernetes/manifests/kube-apiserver.yaml and add the below flags:
```
- --audit-policy-file=/etc/kubernetes/audit-policy.yaml
- --audit-log-path=/var/log/test.log
- --audit-log-maxage=30
```
- Then you need to mount the YAML file to the volume so that the API server can read that file. Also, mount the log path where you need to store the test.log file. For storing the log file you can give the path in the volume mount as shown below:

**Add this in the volume mount section**
```
volumeMounts:
  - mountPath: /etc/kubernetes/audit-policy.yaml
    name: audit
    readOnly: true
  - mountPath: /var/log/test.log
    name: audit-log
    readOnly: false
```
**Add this in the volume section**
```
volumes:
- name: audit
  hostPath:
    path: /etc/kubernetes/audit-policy.yaml
    type: File
- name: audit-log
  hostPath:
    path: /var/log/test.log
    type: FileOrCreate
```
### Execute the below command to create a pod in a different namespace.

- In this case pod will be created  but the logs will be sent to the audit.log file which we specified in the kube-apiserver.yaml.
```
kubectl apply -f privileged-pod.yaml -n prod
```
- In this case, the pod will be created but the logs will be sent to the audit.log file which we specified in the kube-apiserver.yaml.
```
kubectl apply -f baseline-pod.yaml -n prod
```
Execute the below command to see the logs. After execution, the test.log file will be created.
```
tail –f /var/log/test.log
```
