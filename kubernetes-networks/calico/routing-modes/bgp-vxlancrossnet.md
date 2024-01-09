# Mixed Routing BGP / vxlancrossnet

## What does it do ? 

  * BGP is used, when other node is on same subnet
  * vxlan is used, when worker node to reach is in other subnet

## Grafics

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/a2766737-e1e5-4ee0-8e03-9216a0379d97)

## How to find out, if this node is used 

```
kubectl -n calico-system get ippool -o yaml | grep vxlan 
```
