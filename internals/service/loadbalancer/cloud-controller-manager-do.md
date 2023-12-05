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
Der Service Controller ist verantwortlich für das Abhören von Ereignissen zum Erstellen, Aktualisieren und Löschen von Diensten. Basierend auf dem aktuellen Stand der Services in Kubernetes konfiguriert es Cloud Load Balancer (wie ELB, Google LB oder Oracle Cloud Infrastructure LB), um den Zustand der Services in Kubernetes abzubilden. Darüber hinaus wird sichergestellt, dass die Service Backends für Cloud Loadbalancer auf dem neuesten Stand sind.

```


## Load Balancer Implementation in DigitalOcean (DO)

  
  * https://github.com/digitalocean/digitalocean-cloud-controller-manager/tree/master
  * https://github.com/digitalocean/digitalocean-cloud-controller-manager/blob/master/cloud-controller-manager/do/loadbalancers.go

### api - domain is hardcoded in cloud controller manager for digitalocean  

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/d3b2d698-9fcb-4a46-981e-6bb38067aadc)

## References:

  * [Good explanation](https://medium.com/@m.json/the-kubernetes-cloud-controller-manager-d440af0d2be5)
  * [Zugrundeliegende Konzepte](https://kubernetes.io/de/docs/concepts/architecture/cloud-controller/)
