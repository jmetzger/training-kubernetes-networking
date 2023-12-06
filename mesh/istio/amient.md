# Istio without sidecar (ambient)

## the istio-cni 

## To use ambient you will just label the namespace 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/ee85b46e-f8f3-4bb3-b458-55083959083e)

## Traffic redirection through ztunnels 

```
By default, the ambient profile has the Istio core, Istiod, ingress gateway, zero-trust tunnel agent (ztunnel) and CNI plugin enabled.

The Istio CNI plugin is responsible for detecting which application pods are part of the ambient mesh and configuring the traffic redirection between the ztunnels.
```
