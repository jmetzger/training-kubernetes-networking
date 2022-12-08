# Networking Internals Overview 

## Network Namespace for each pod 

### Overview 

![Overview]()

### General 

  * Each pod will have its own network namespace
    * with routing, networkdevices 
  * Connection to default namespace to host is done through veth - Link to bridge on host network 
    * similar like on docker to docker0 
  * Every container is in the same Network Namespace, so they can communicate through localhost
    * Example with hashicorp/http-echo container 1 and busybox container 2 ? 
 


   
    


## Hidden Pause Container 

### What is for ? 

  * Holds the network - namespace for the pod 
  * Gets started first and falls asleep later 
  * Will still be there, when the other containers die 

```
cd 
mkdir -p manifests 
cd manifests 
mkdir pausetest
cd pausetest
nano 01-nginx.yml
```

```
# vi nginx-static.yml 

apiVersion: v1
kind: Pod
metadata:
  name: nginx-pausetest
  labels:
    webserver: nginx:1.21
spec:
  containers:
  - name: web
    image: nginx
```

```
kubectl apply -f .

ctr -n k8s.io c list | grep pause
```
