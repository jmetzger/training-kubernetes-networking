# Find corresponding interfaces 

## Walkthrough  (without calicoctl)

```bash
# Step 1: create pod 
kubectl run nginx-master --image=nginx
# Find out on which node it runs 
kubectl get pods -o wide 
# create a debug container 
kubectl debug -it nginx-master --image=busybox 
```

```
# now within debug pod found out interface 
ip a | grep @
```

```
# Ausgabe 
3: eth0@if22: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue
```

```
# Log in to worker node  where pod runs and check interfaces
kubectl debug -it node/worker1 --image=busybox
```

```
# on worker node 
# show matched line starting with 22 and then another 4 lines 
ip a | grep -A 5 ^22 
# e.g. 
# 
ip a | grep -A 5 ^22
22: cali42c2aab93f3@if3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff link-netns cni-5adf994b-3a7e-c344-5d82-ef1f7a293d88
    inet6 fe80::ecee:eeff:feee:eeee/64 scope link
       valid_lft forever preferred_lft forever
```

## Get information with calicoctl (installed on client) 

```
# für den namespace defaut bzw. den konfigurierten 
calicoctl get wep
calicoctl get workloadendpoints

# für alle namespaces
calicoctl get wep -A 
```


## Firewall - Regeln 

```
# Now you are able to determine the firewall rules 
# you will find fw and tw rules (fw - from workload and tw - to workload)
iptables-legacy -L -v | grep  cali42c2aab93f3
```

```
# ... That is what you see as an example 
Chain cali-tw-cali42c2aab93f3 (1 references)
 pkts bytes target     prot opt in     out     source               destination 
   10  1384 ACCEPT     all  --  any    any     anywhere             anywhere             /* cali:WKA8EzdUNM0rVty1 */ ctstate RELATED,ESTABLISHED
    0     0 DROP       all  --  any    any     anywhere             anywhere             /* cali:wr_OqGXKIN_LWnX0 */ ctstate INVALID
    0     0 MARK       all  --  any    any     anywhere             anywhere             /* cali:kOUMqNj8np60A3Bi */ MARK and 0xfffeffff
```


