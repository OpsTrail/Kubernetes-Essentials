# create Bastion instance

| Purpose   | VM Name          | CPU | Memory | Disk  | Operating System |
| -------   | ---------------- | --- | ------ | ----  | ---------------- |
| Machine 1 | EKS-bastion              |  1  | 2 GB   | 10 GB | Ubuntu           |

### Install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.12.3.zip" -o "awscliv2.zip"
apt install unzip
unzip awscliv2.zip
./aws/install
```
### Install eksctl
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/v0.193.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp 
sudo mv /tmp/eksctl /usr/bin
eksctl version
```
### Install Kubectl tool
```
curl -LO https://dl.k8s.io/release/v1.29.9/bin/linux/amd64/kubectl
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x /usr/bin/kubectl
kubectl version --client
```
### Install aws iam authenticator
```
curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/aws-iam-authenticator
chmod +x ./aws-iam-authenticator
sudo mv ./aws-iam-authenticator /usr/bin
```
### Install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
### Create thecluster
```
eksctl create cluster -f cluster.yaml
```
### Update the kubeconfig and connect with the cluster
```
aws eks update-kubeconfig --region ap-south-1 --name demo-cluster
```



