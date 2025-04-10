# Debug as root using iptables-legacy (for iptables entries in kernel) 


```
kubectl debug node/worker2 -it --image=nicolaka/netshoot --profile=netadmin  
# in der shell 
apk add iptables-legacy
```
