# Implementation of type:LoadBalancer in cloud-controller-manager.do 

## Keypart: Cluster Controller Manager (CCM) 

  * was decoupled from Kube Controller Manager
    * to make it easier for cloud providers to implement their specific environment/workings (e.g. LoadBalancer)
  * To do this a skeleton was provided.

![CCM](https://kubernetes.io/images/docs/post-ccm-arch.png)

## Example of skeleton for LoadBalancer:



![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/d3b2d698-9fcb-4a46-981e-6bb38067aadc)
