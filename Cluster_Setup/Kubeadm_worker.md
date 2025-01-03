### Disable swap

- First, we are disabling the swap and removing the entries from the fstab

```
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
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
modprobe overlay
modprobe br_netfilter
```

### Configuring System Networking

- Here it is Ensuring that the Linux bridge passes the ipv4 and iv6 network traffic to iptables for filtering. And it is also Enabling packet forwarding for IPv4, which is necessary for pod-to-pod communication across nodes.

```  
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```
- After that apply the above sysctl configurations without rebooting the system.

```
sysctl --system
```

### Adding Kubernetes repository

- Before adding the Kubernetes repository we are doing apt update and and installing some packages

```
apt-get update
apt-get install -y apt-transport-https ca-certificates curl gpg

mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

### Installing Kubeadm, Kubectl, Kubelet

```
apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

### Adding Docker Repository

```
apt-get update
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### Installing Docker and Containerd

- Containerd is one of the Container Runtime Interface(CRI) for kubernetes to run the containers.

```
apt-get update
apt-get install containerd.io docker-ce docker-ce-cli docker-buildx-plugin -y
```
### Configuring Systemd Cgroup for contanerd

- Basically, it Generates a default containerd configuration and modifies it to use systemd as the cgroup driver, which is recommended for Kubernetes.

```
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml
sed -e 's/SystemdCgroup = false/SystemdCgroup = true/g' -i /etc/containerd/config.toml
```
### Restarting Containerd

- Restarting and enablig the containerd service so the above configuration can be applied.

```
systemctl restart containerd
systemctl enable containerd
systemctl status containerd
```  
### Kubeadm join command

- This join command you need to copy from the output of the kubeadm init command and add in the worker node script

```
kubeadm join 172.31.47.208:6443 --token jmyi6c.vfidjauflvpuebns --discovery-token-ca-cert-hash sha256:76c409bc97f0a5cf5b14cd30705cf84b250eb90835aab70b734e34f14c2e9628
```
