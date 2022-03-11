# Kubernetes Netzwerk (CNI) 

## CNI 

  * Common Network Interface
  * Fest Definition, wie Container mit Netzwerk-Bibliotheken kommunizieren

## Docker - Container oder andere 

  * Container wird hochgefahren -> über CNI -> zieht Netzwerk - IP  hoch. 
  * Container witd runtergahren -> uber CNI -> Netzwerk - IP wird released 

## Welche gibt es ? 

  * Flanell 
  * Canal 
  * Calico 
  
## Flannell

### Overlay - Netzwerk 

  * virtuelles Netzwerk was sich oben drüber und eigentlich auf Netzwerkebene nicht existiert
  * VXLAN 

### Vorteile 

  * Guter einfacher Einstieg 
  * redziert auf eine Binary flanneld 

### Nachteile 

  * keine Firewall - Policies möglich 
  * keine klassichen Netzwerk-Tools zum Debuggen möglich. 

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
