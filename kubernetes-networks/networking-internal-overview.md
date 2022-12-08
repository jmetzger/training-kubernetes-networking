# Networking Internals Overview 

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

ctr -n k8s.io c list 
```
