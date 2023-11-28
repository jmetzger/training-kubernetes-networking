# kube-proxy 

## Basics 

  * Runs on localhost:10249
  * Has some Endpoints e.g. proxyMode



## ProxyModes 

  * UserSpace
  * iptables
  * ipvs (Virtual Server)
    * https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/
    * Direkt im Kernel eingebautes LoadBalancing auf L4 (transport layer)

## Wie finde icn den ProxyMode heraus ? 

 ![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/31ff3ae4-7b30-4ca1-908d-38b83afd1b7f)

  * Alternative: kubectl logs des-proxy-pods

## UserSpace 

  * Auf dem Kube-Proxy werden für die jeweilige ClusterIP - Ports geöffnet und dort erfolgt dann über NAT-Regeln eine Weiterleitung zu den Pods 
  * Nachteil: sehr unperformant, da im UserSpace
  * Ist historisch und wird eigentlich nicht mehr verwendet. 

## iptables 

  * Besser als UserSpace / meistens default auf den Kube-Clustern aktuell
  * Regeln werden zwar vom kube-proxy initiiert, aber schreiben direkt NAT-Regeln, ohne Umwege über den Kube-Proxy 
  * Skaliert nicht gut (siehe IPVS)
  * Auf jedem Node werden für jede ClusterIP und jeden NodePort - Regeln geschrieben. (siehe: Wie ist der Ablauf ?) 
  * je mehr Services, desto mehr iptables - Regeln

## IPVS ()

  * Reference: https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/
   
