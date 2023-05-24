# Logging firewall calico 

## General

 * NetworkPolicy of Kubernetes does not provide possibility to track 

## Solutions 

  * Use NetworkPolicy from calico (to apply it with kubectl - the calico api server needs to be installed) / or use calicoctl 
  * Enable Tracing 
  * Use: https://kubernetes.io/blog/2019/04/19/introducing-kube-iptables-tailer/
  
## Solution 1: NetworkPolicy calico 

  * https://github.com/projectcalico/calico/issues/4344


## Logs 

```
# Normally you should see it with (on the right kubernetes node)
cat /var/log/syslog | grep calico-packet 

# This is how a syslog entry looks like 
Here is a example (default) Log:
Apr  3 10:12:30 aks-workerpool1-13987120-vmss000000 kernel: [10821.860593] calico-packet: IN=calic440f455693 OUT=eth0 MAC=ee:ee:ee:ee:ee:ee:f2:f8:09:3d:97:03:08:00 SRC=10.244.2.7 DST=8.8.8.8 LEN=84 TOS=0x00 PREC=0x00 TTL=63 ID=33536 DF PROTO=ICMP TYPE=8 CODE=0 ID=32113 SEQ=43 
```

## Next example 

```
cd
mkdir -p manifests
cd manifests 
mkdir pol2
cd pol2
vi 01-pod.yaml 
```

```
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    app: web
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
```

```
vi 02-pol.yaml 
```

```
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: log
spec:
  selector: app == 'web'
  types:
  - Ingress
  - Egress
  ingress:
  - action: Log
  egress:
  - action: Log
  - action: Deny
```

```
kubectl apply -f .
# find the node, where it runs on 
kubectl get pods -o wide 
```

```
# login to that node with ssh (kubernetes node) 
# e.g. ssh user@node 
# switch to root: sudo su -
tail -f /var/log/syslog | grep calico-packet 
# or 
journalctl -f | grep calico-packet 
```

```
# now open a debug pod 
kubectl debug -it static-web --target --image=busybox 
# in pod ping - this will not work, because we cannot retrieve dns 
ping www.google.de
```

```
# watch output from other node in the meanwhile 
```

