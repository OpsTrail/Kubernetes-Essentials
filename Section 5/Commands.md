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
touch file3.txt
```
### Now Go to the worker node.

 - Check the directory which you have created and do ls you will find the files which you have created.
