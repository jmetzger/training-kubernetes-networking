# Kubernetes Netzwerk (CNI) 

## CNI 

  * Common Network Interface
  * Fest Definition, wie Container mit Netzwerk-Bibliotheken kommunizieren

## Docker - Container oder andere 

  * Container wird hochgefahren -> über CNI -> zieht Netzwerk - IP  hoch. 
  * Container witd runtergahren -> uber CNI -> Netzwerk - IP wird released 

## Welche gibt es ? 

  * Flannell 
  * Canal 
  * Callisto 
  
## Flannell

### Overlay - Netzwerk 

 * virtuelles Netzwerk was sich oben drüber und eigentlich auf Netzwerkebene nicht exists 
 * VXLAN 

### Vorteile 

 * Guter einfacher Einstieg 
 * redziert auf eine Binarry flanneld 

## Nachteile 

 * keine Firewall - Policies möglich 
 * keine klassichen Netzwerk-Tools zum Debuggen möglich. 

