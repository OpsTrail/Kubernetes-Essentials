### Use-Case: Checking Redis Server status with init container

- For this use, we are creating a custom image in which we are using alpine as base image and adding Redis tools to use the Redis CLI. Then from the command args we are running one while loop that will check for Redis service on port 6379 and send the ping request in every 10 seconds once it gets the pong response, it will allow the main container to start.

- Then we will apply the redis.yaml file so that the init container gets executed successfully.

- **So First you need to apply init-pod.yaml**
```
Kubectl apply -f init-pod.yaml
```
- **After that execute the redis.yaml**
```
kubectl apply -f redis.yaml
```
