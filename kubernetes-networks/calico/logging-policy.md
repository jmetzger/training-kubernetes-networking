# Logging firewall calico 

## General

 * NetworkPolicy of Kubernetes does not provide possibility to track 

## Solutions 

  * Use NetworkPolicy from calico (to apply it with kubectl - the calico api server needs to be installed) / or use calicoctl 
  * Enable Tracing 
  * Use: 
  
## Solution 1: NetworkPolicy calico 

  * https://github.com/projectcalico/calico/issues/4344

```
cd 
mkdir manifests 
cd manifests 
mkdir cpol 
cd cpol 
vi 04-pol.yaml 
```

```
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: hack-a
spec:
  ingress:
  - action: Log
    destination: {}
    protocol: TCP
    source: {}
  - action: Deny
    destination: {}
    protocol: TCP
    source: {}
  types:
  - Ingress
  - Egress
```

```
kubectl create ns hack-a 
kubectl apply -f . 
```

```
# create nginx-pod in namespace hack-a 
kubectl -n hack-a run nginx-test --image=nginx-test
# obtain pod-ip 
kubectl -n hack-a get pods -o wide 

# run a busybox to test connection 
kubectl -n hack-a podtester --rm --image=busybox 
```


## Logs 

```
# Normally you should see it with (on the right kubernetes node)
cat /var/log/syslog | grep calico-packet 

# This is how a syslog entry looks like 
Here is a example (default) Log:
Apr  3 10:12:30 aks-workerpool1-13987120-vmss000000 kernel: [10821.860593] calico-packet: IN=calic440f455693 OUT=eth0 MAC=ee:ee:ee:ee:ee:ee:f2:f8:09:3d:97:03:08:00 SRC=10.244.2.7 DST=8.8.8.8 LEN=84 TOS=0x00 PREC=0x00 TTL=63 ID=33536 DF PROTO=ICMP TYPE=8 CODE=0 ID=32113 SEQ=43 
```
