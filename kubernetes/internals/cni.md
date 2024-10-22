# CNI 




## Referenz:

  * https://isovalent.com/blog/post/demystifying-cni/


## Ablauf 
   * Containerd ruft CNI plugin über subcommandos: ADD, DEL, CHECK, VERSION auf (mehr subcommandos gibt es nicht)
   * Was gemacht werden soll wird über JSON-Objekt übergeben
   * Die Antwort kommt auch wieder als JSON zurück 

## Plugins die Standardmäßig schon da sind 
 
   * https://www.cni.dev/plugins/current/

## CNI-Provider 

   * Ein Kubernetes-Cluster braucht immer ein CNI-Provider, sonst funktioniert die Kmmunikation nicht und die Nodes im Cluster stehen auf NotReady 
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

## Beispiel json für antrea (wird verwendet beim Aufruf von CNI) 

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/85dcbcf4-0c01-4fe0-a737-dd0f7d04231f)
