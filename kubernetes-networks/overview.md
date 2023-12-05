# Kubernetes Netzwerk (CNI) 

## CNI 

  * Common Network Interface
  * Feste Definition, wie Container mit Netzwerk-Bibliotheken kommunizieren

## Docker - Container oder andere 

  * Container wird hochgefahren -> über CNI -> zieht Netzwerk - IP  hoch. 
  * Container witd runtergahren -> uber CNI -> Netzwerk - IP wird released 

## Welche gibt es ? 

  * Flanel
  * Canal 
  * Calico 
  * Cilium
  * Antrea (vmware)
  * Weave Net 
  
## Flannel

### Overlay - Netzwerk 

  * virtuelles Netzwerk was sich oben drüber und eigentlich auf Netzwerkebene nicht existiert
  * VXLAN

### Vorteile 

  * Guter einfacher Einstieg 
  * reduziert auf eine Binary flanneld 

### Nachteile 

  * keine Firewall - Policies möglich 
  * keine klassichen Netzwerk-Tools zum Debuggen möglich. 

### Guter Einstieg in flannel 

  * https://mvallim.github.io/kubernetes-under-the-hood/documentation/kube-flannel.html

## Canal 

### General 

  * Auch ein Overlay - Netzwerk 
  * Unterstüzt auch policies 

## Calico

### Generell 

  * klassische Netzwerk (BGP)

### Vorteile gegenüber Flannel 

  * Policy über Kubernetes Object (NetworkPolicies)

### Vorteile 

  * ISTIO integrierbar (Mesh - Netz) 
  * Performance etwas besser als Flannel (weil keine Encapsulation)

### Referenz 
  * https://projectcalico.docs.tigera.io/security/calico-network-policy

## Cilium 

![Cilium Architecture](https://docs.cilium.io/en/stable/_images/cilium-arch.png)

### Generell 

  * Verwendet keine Bridge sondern Hooks im Kernel, die mit eBPF aufgesetzt werden
    * Bessere Performance
  * eBPF wird auch für NetworkPolicies unter der Haube eingesetzt
  * Mit Ciliums Cluster Mesh lassen sich mehrere Cluster miteinander verbinden:

### Vorteile 

  * Höhere Leistung mit eBPF-Ansatz. (extended Berkely Packet Filter)
    * JIT - Just in time compiled -
    * Bytecode wird zu MaschineCode kompiliert (Miniprogramme im Kernel)
  * Ersatz für iptables (wesentlich schneller und keine Degredation wie iptables ab 5000 Services)
  * Gut geeignet für größere Cluster 

## Weave Net 

  * Ähnlich calico 
  * Verwendet overlay netzwerk
  * Sehr stabil bzgl IPV4/IPV6 (Dual Stack) 
  * Sehr grosses Feature-Set 
  * mit das älteste Plugin 


## microk8s Vergleich 

  * https://microk8s.io/compare

```
snap.microk8s.daemon-flanneld
Flannel is a CNI which gives a subnet to each host for use with container runtimes.

Flanneld runs if ha-cluster is not enabled. If ha-cluster is enabled, calico is run instead.

The flannel daemon is started using the arguments in ${SNAP_DATA}/args/flanneld. For more information on the configuration, see the flannel documentation.
```
