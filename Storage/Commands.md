# EmptyDir

### Create the YAML file

```
kubectl apply -f emptydir.yaml
```

### Exec into the pod

- Go to the directory which you have mounted 
```
cd /data/
```
- And create some files and exit
```
touch file1.txt
touch file2.txt
```
### Now Go to the worker node and List the container

- Since we are using containerd as our CRI we need to use ctr command for containerd and the container are created under k8s.io namespace by default.
```
ctr -n k8s.io container list
```

- Inspect the container. You can find the mounted directory and the source of the temporary created directory. You can go that directory and do ls you will find the files which you have created.
```
ctr -n k8s.io containers info <container id>
```

# HostPath

### Create the YAML file

```
kubectl apply -f hostpath.yaml
```

### Exec into the pod

- Go to the directory which you have mounted 
```
cd /data/
```
- And create some files and exit
```
touch file1.txt
touch file2.txt
touch file3.txt
```
### Now Go to the worker node.

 - Check the directory which you have created and do ls you will find the files which you have created.

# PV and PVC

### Create the PV and PVC
- You can create the PV and PVC with the YAML file.
- You can also separate the configuration of PV and PVC and run from different files. 
```
kubectl apply -f pv-pvc.yaml
```

### Creaate the pod climing the PVC
```
kubectl apply -f pod-pvc.yaml
```

### List the PV
```
kubectl get PV
```

### List the PVC
```
kubectl get PVC
```

### Describe the PV
```
kubectl describe PV <pv name>
```

### Describe the PVC
```
kubectl describe PVC <pvc name>
```

### Delete the PV
- By specifying the pv name
```
kubectl delete pv <pv name>
```
- By specifying the YAML file
```
kubectl delete -f pv.yaml
```

### Delete the PVC
- You cannot delete the pvc if any pod is using it.

```
kubectl delete PVC <PVC name>
```

**To delete the PVC which stuck in the terminating state**

- The same can be done with the PV as well
```
kubectl patch pvc {PVC_NAME} -p '{"metadata":{"finalizers":null}}'
```
Here Fin
# Storage Class

### Install the provisioner driver for the storage class

- You can install the latest rancher local path driver using the link below.

```
https://github.com/rancher/local-path-provisioner?tab=readme-ov-file#installation
```

- Or you can directly apply the file or download the file using wget followed by the git URL given in the link below.

```
wget https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.29/deploy/local-path-storage.yaml
```
- After that you can apply the file this will create the storage class as well.
```
kubectl apply -f local-path-storage.yaml
```

### list the storage class
```
kubectl get sc
```

### Describe the storage class
```
kubectl describe sc <sc name>
```

### Create the Storage class
- it can be create through yaml files
```
kubectl apply -f <file name.yaml>
```

### Creat PVC with storage class name
```
kubectl apply -f pvc-sc.yaml
```

### Create a Pod claiming the PVC
```
kubectl apply -f pod-pv.yaml
```

### Delete Storage class

- if you want to delete the storage you can use the below command
```
kubectl delete sc <sc name>
```
