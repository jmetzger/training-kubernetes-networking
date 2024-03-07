# Wann calicoctl (Stand 2024-01 Version calico 3.27) 

## Für Informationen über die Nodes (z.B. BGP) - direkt auf Node ausführen 

 * calicoctl get nodes

## Um Zusatzinformationen abzufragen, die nur in calicoctl zur Verfügung stehen 

```
# namespace in command needs to be written at then end
calicoctl get wep -n namespace-der-application

# get version
calicoctl version

# show cidr / the ippool 
calicoctl ipam show
calicoctl ipam check 

```

## Calico - only on one of nodes (e.g. controlplane - need to login with ssh) 

```
# .kube/config does not need to be configured 
calicoctl node status
calicoctl ipam status 


```
