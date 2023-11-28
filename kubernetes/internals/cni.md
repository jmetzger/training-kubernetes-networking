# CNI 

## Ablauf 
   * Containerd ruft CNI plugin über subcommandos: ADD, DEL, CHECK auf (mehr subcommandos gibt es nicht)
   * Was gemacht werden soll wird über JSON-Objekt übergeben
   * Die Antwort kommt auch wieder als JSON zurück 

## Plugins die Standardmäßig schon da sind 
 
   * https://www.cni.dev/plugins/current/

## CNI-Plugin: 

   * Ein Kubernetes-Cluster braucht immer ein CNI-Plugin, sonst funktioniert die Kmmunikation nicht und die Nodes im Cluster steht auf NotReady 
   * Beispiele: Calico, WeaveNet, Antrea, Cilium, Flannel 

## IPAM - IP Address Management 

   * Ziel ist, dass Adressen nicht mehrmals vergeben werden.
   * Dazu wird ein Pool bereitgestellt.
   * Es gibt 3 CNI IPAM - Module:
     * host-local
     * dhcp
     * static  
```
* IPAM: IP address allocation 
dhcp : Runs a daemon on the host to make DHCP requests on behalf of a container
host-local : Maintains a local database of allocated IPs
static : Allocates static IPv4/IPv6 addresses to containers
```
