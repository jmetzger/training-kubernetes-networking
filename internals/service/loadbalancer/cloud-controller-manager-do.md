# Implementation of type:LoadBalancer in cloud-controller-manager.do 

## Keypart: Cluster Controller Manager (CCM) 

  * was decoupled from Kube Controller Manager
    * to make it easier for cloud providers to implement their specific environment/workings (e.g. LoadBalancer)
  * To do this a skeleton was provided.

![CCM](https://kubernetes.io/images/docs/post-ccm-arch.png)

## Control Loops in the CCM 

  * Der CCM erbt seine Funktionen von Komponenten des Kubernetes, die von einem Cloud Provider abhängig sind.
  * Die meisten Funktionen des CCM stammen aus dem KCM. Wie im vorherigen Abschnitt erwähnt, führt das CCM die folgenden Steuerschleifen durch:

```
Node Controller
Route Controller
Service Controller
```

## Service Controller 

```


```


## Example of skeleton for LoadBalancer:


  * https://github.com/digitalocean/digitalocean-cloud-controller-manager/tree/master

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/d3b2d698-9fcb-4a46-981e-6bb38067aadc)
