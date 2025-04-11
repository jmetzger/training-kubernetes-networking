# Multus 

## Problem, Warum multus ? 

  * Aktuell kann seitens kubernetes nur ein Interface verwaltet werden, weil der CNI-Call nur 1x ausgeführt wird. (eigentlich 2x wenn man localhost mit einbezieht)

## Prerequisites 

  * a CNI must be installed beforehand, that manages the network (Calico, Cilium)

## Graphics 

![Multus](https://github.com/k8snetworkplumbingwg/multus-cni/raw/master/docs/images/multus-pod-image.svg)

## General 

  * Multus is a meta-plugin, which makes it possible to attach additional networks to your pod (multi - homing)

## macvlan plugin 

## Example macvlan 

  * https://github.com/k8snetworkplumbingwg/multus-cni/blob/master/examples/macvlan-pod.yml

```
mkdir -p manifests/multus-example
cd manifests/multus-example
nano network-attachment.yaml
```


```
---
# This net-attach-def defines macvlan-conf with 
#   + ips capabilities to specify ip in pod annotation and 
#   + mac capabilities to specify mac address in pod annotation
# default gateway is defined as well
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: macvlan-conf
spec: 
  config: '{
      "cniVersion": "0.3.1",
      "plugins": [
        {
          "type": "macvlan",
          "capabilities": { "ips": true },
          "master": "eth1",
          "mode": "bridge",
          "ipam": {
            "type": "static",
            "routes": [
              {
                "dst": "0.0.0.0/0",
                "gw": "10.1.1.1"
              }
            ] 
          }
        }, {
          "capabilities": { "mac": true },
          "type": "tuning"
        }
      ]
    }'
```

```
kubectl apply -f .
```


```
nano pod.yaml
```

```
# Define a pod with macvlan-conf, defined above, with ip address and mac, and 
# "gateway" overrides default gateway to use macvlan-conf's one. 
# without "gateway" in k8s.v1.cni.cncf.io/networks, default route will be cluster
# network interface, eth0, even tough macvlan-conf has default gateway config.
apiVersion: v1
kind: Pod
metadata:
  name: samplepod
  annotations:
    k8s.v1.cni.cncf.io/networks: '[
            { "name": "macvlan-conf",
              "ips": [ "10.1.1.101/24" ],
              "mac": "c2:b0:57:49:47:f1",
              "gateway": [ "10.1.1.1" ]
            }]'
spec:
  containers:
  - name: samplepod
    command: ["/bin/bash", "-c", "trap : TERM INT; sleep infinity & wait"]
    image: dougbtv/centos-network
    ports:
    - containerPort: 80

```

```
kubectl apply -f .
```
