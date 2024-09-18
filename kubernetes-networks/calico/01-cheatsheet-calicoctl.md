# calicoctl cheatsheets 

## vom client aus 

```
calicoctl ipam show
calicoctl ipam show --show-blocks
calicoctl ipam check 

calicoctl get felixconfiguration
calicoctl get felixconfiguration default -o yaml
# Wird bgp verwendet 
kubectl -n calico-system logs ds/calico-node  | grep -i bgp

## von der node aus / dort calicoctl installieren 
calicoctl node status 
```


```
IPv4 BGP status
+--------------+-------------------+-------+------------+-------------+
| PEER ADDRESS |     PEER TYPE     | STATE |   SINCE    |    INFO     |
+--------------+-------------------+-------+------------+-------------+
| 10.135.0.10  | node-to-node mesh | up    | 2024-09-17 | Established |
| 10.135.0.29  | node-to-node mesh | up    | 2024-09-17 | Established |
| 10.135.0.12  | node-to-node mesh | up    | 2024-09-17 | Established |
+--------------+-------------------+-------+------------+-------------+
```
