# Which routing mode is used 

```
kubectl -n calico-system describe ds calico-node | grep -A 35  calico-node
# or specific
kubectl -n calico-system describe ds calico-node | egrep -i -e vxlan  -e cluster_type

```

```
Environment:
      DATASTORE_TYPE:                      kubernetes
      WAIT_FOR_DATASTORE:                  true
      CLUSTER_TYPE:                        k8s,operator,bgp
      CALICO_DISABLE_FILE_LOGGING:         false
      FELIX_DEFAULTENDPOINTTOHOSTACTION:   ACCEPT
      FELIX_HEALTHENABLED:                 true
      FELIX_HEALTHPORT:                    9099
      NODENAME:                             (v1:spec.nodeName)
      NAMESPACE:                            (v1:metadata.namespace)
      FELIX_TYPHAK8SNAMESPACE:             calico-system
      FELIX_TYPHAK8SSERVICENAME:           calico-typha
      FELIX_TYPHACAFILE:                   /etc/pki/tls/certs/tigera-ca-bundle.crt
      FELIX_TYPHACERTFILE:                 /node-certs/tls.crt
      FELIX_TYPHAKEYFILE:                  /node-certs/tls.key
      FIPS_MODE_ENABLED:                   false
      FELIX_TYPHACN:                       typha-server
      CALICO_MANAGE_CNI:                   true
      CALICO_IPV4POOL_CIDR:                192.168.0.0/16
      CALICO_IPV4POOL_VXLAN:               CrossSubnet
      CALICO_IPV4POOL_BLOCK_SIZE:          26
      CALICO_IPV4POOL_NODE_SELECTOR:       all()
      CALICO_IPV4POOL_DISABLE_BGP_EXPORT:  false
      CALICO_NETWORKING_BACKEND:           bird
      IP:                                  autodetect
      IP_AUTODETECTION_METHOD:             first-found
      IP6:                                 none
      FELIX_IPV6SUPPORT:                   false
      KUBERNETES_SERVICE_HOST:             10.96.0.1
      KUBERNETES_SERVICE_PORT:             443
    Mounts:
```
