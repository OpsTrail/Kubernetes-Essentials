# Upgrade Master Node
- Run the apt update command and then run the Madison command to list the latest version.
```
apt update
```
```
apt-cache madison kubeadm
```
**NOTE: Make sure that the upgrade procedure should be done on one node at a time**

### Upgrading the control Plane Node
- Unhold the kubeadm
```
apt-mark unhold kubeadm
```
- Install the latest version of kubeadm
```
apt-get update && sudo apt-get install -y kubeadm='1.31.1'
```
- After installing the latest version mark kubeadm as hold.
```
apt-mark hold kubeadm
```
- Run the plan command. This command will show you that your cluster can be upgraded and fetch the latest version to which the cluster will be upgraded. It will also show the components with their version states.
```
kubeadm upgrade plan
```
- To run the upgrade for you kubeadm
```
kubeadm upgrade apply v1.31.1
```
- Now if you do get nodes
```
kubectl get nodes
```
- you will see the previous version  even though you have upgraded the kubeadm. This is because we also need to upgrade the kubelet configuration to the latest version so that it can register the new version of your cluster.

### Drain the master node
- Before upgrading the kubelet we need to drain the master node and make the node un-schedulable. During this time master node components will be unavailable but your workload will not get impacted because kubelet will be managing those pods on the worker node. 
```
kubectl drain <node-to-drain> --ignore-daemonsets
```
- After that mark the kubelet and kubectl unhold
```
apt-mark unhold kubelet kubectl
```
- Run the apt update and install the latest version of the kubelet and kubectl
```
apt-get update && apt-get install -y kubelet='1.31.1' kubectl='1.31.1'
```
- After installing mark the kubectl and kubelt on hold
 ``` 
apt-mark hold kubelet kubectl
```
- Restart the kubelet
```
systemctl daemon-reload
systemctl restart kubelet
```
### Uncordon the node
- After updating the kubelet and kubectl uncordon the node to make it schedulable.
```
kubectl uncordon <node-to-uncordon>
```
- once you uncordon the node the control plane components will come into running state.

# Upgrade Worker Node
- Login into your worker node
- once you login mark the kubeadm as unhold and update the kubeadm in worker node as well
```
apt-mark unhold kubeadm
```
- Update the kubeadm
```
apt-get update && apt-get install -y kubeadm='1.31.1'
```
- Mark as hold for kubeadm
```
apt-mark hold kubeadm
```
- Upgrade the kubelet configuration in the worker node.
```
kubeadm upgrade node
```
**Now go to the Master node and run the drain command to drain the worker node**
```
kubectl drain <node-to-drain> --ignore-daemonsets
```
**Now come back to worker node**
- Upgrade the Kubelet and kubectl in worker node
``` 
apt-mark unhold kubelet kubectl
```
```
apt-get update && apt-get install -y kubelet='1.31.1' kubectl='1.31.1'
```
```
apt-mark hold kubelet kubectl
```
- Restart the kubelet
```
systemctl daemon-reload
systemctl restart kubelet
```
- After the upgrade uncordon the node to schedule the pods
```
kubectl uncordon <node-to-uncordon>
```
- Now if you do get node you will get the latest version of the Kubernetes.
- To reschedule the deployment evicted pod you can restart the deployment to reschedule the pod on the node.
```
kubectl rollout restart deployment <deployment-name>
```
