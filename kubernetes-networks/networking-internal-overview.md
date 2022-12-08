# Networking Internals Overview 

## Network Namespace for each pod 

### Overview 

![Overview](https://www.inovex.de/wp-content/uploads/2020/05/Container-to-Container-Networking_2_neu-400x401.png)
![Overview Kubernetes Networking](https://www.inovex.de/wp-content/uploads/2020/05/Container-to-Container-Networking_3_neu-400x412.png)

### General 

  * Each pod will have its own network namespace
    * with routing, networkdevices 
  * Connection to default namespace to host is done through veth - Link to bridge on host network 
    * similar like on docker to docker0 
  
```
  Each container is connected to the bridge via a veth-pair. This interface pair functions like a virtual point-to-point ethernet connection and connects the network namespaces of the containers with the network namespace of the host
```
  
  * Every container is in the same Network Namespace, so they can communicate through localhost
    * Example with hashicorp/http-echo container 1 and busybox container 2 ? 
 
 
## Pod-To-Pod Communication (across nodes)  
 
### Prerequisites 
 
  * pods on a single node as well as pods on a topological remote can establish communication at all times
   * Each pod receives a unique IP address, valid anywhere in the cluster. Kubernetes requires this address to not be subject to network address   translation (NAT)
   * Pods on the same node through virtual bridge (see image above)
 
### General (what needs to be done) - and could be doen manually
 
   * local bridge networks of all nodes need to be connected
   * there needs to be an IPAM (IP-Address Managemenet) so addresses are only used once
   * The need to be routes so, that each bridge can communicate with the bridge on the other network
   * Plus: There needs to be a rule for incoming network
   * Also: A tunnel needs to be set up to the outside world.

### General - Pod-to-Pod Communiation (across nodes) 

![pod to pod across nodes](https://www.inovex.de/wp-content/uploads/2020/05/Pod-to-Pod-Networking.png)

   

   
    


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


## References 

  * https://www.inovex.de/de/blog/kubernetes-networking-part-1-en/
  * 
