### Disable swap

- First, we are disabling the swap and removing the entries from the fstab

```
swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```
### Loading Required Kernel Modules

- overlay is used for storage management for cuntainer runtimes
netfilter is used for allowing iptables to interact with network bridge traffic

```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```
```
sudo modprobe overlay
sudo modprobe br_netfilter
```

### Configuring System Networking

- Here it is Ensuring that the Linux bridge passes the ipv4 and iv6 network traffic to iptables for filtering. And also Enabling packet forwarding for IPv4, which is necessary for pod-to-pod communication across nodes.

```  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```
- After that apply the above sysctl configurations without rebooting the system.

```
sudo sysctl --system
```

### Adding Kubernetes repository

- Before adding the Kubernetes repository we are doing apt update and and installing some packages

```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

### Installing Kubeadm, Kubectl, Kubelet

```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### Adding Docker Repository

```
sudo apt-get update
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### Installing Docker and Containerd

```
sudo apt-get update
sudo apt-get install containerd.io docker-ce docker-ce-cli docker-buildx-plugin -y
```
### Configuring Systemd Cgroup for contanerd

- Basically here it is Generating a default containerd configuration and modifies it to use systemd as the cgroup driver, which is recommended for Kubernetes.

```
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
sed -e 's/SystemdCgroup = false/SystemdCgroup = true/g' -i /etc/containerd/config.toml
```
### Restarting Containerd

```
sudo systemctl restart containerd
sudo systemctl enable containerd
sudo systemctl status containerd
```  
### Initializing Kubeadm

- Using Kubeadom Initializing the Kubernetes control plane on the master node and Defining the CIDR block for the pod network.

```
kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr=192.168.0.0/16
```
### Configure kubeconfig for the User

- Configure kubeconfig for the Ubuntu user to use the kubectl without sudo.

```
sudo mkdir -p /home/ubuntu/.kube
sudo cp /etc/kubernetes/admin.conf /home/ubuntu/.kube/config
sudo chown -R ubuntu:ubuntu /home/ubuntu/.kube/config
export KUBECONFIG=/home/ubuntu/.kube/config
```

### Installing Calico CNI
```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/calico.yaml
```
