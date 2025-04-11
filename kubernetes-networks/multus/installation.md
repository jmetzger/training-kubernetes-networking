# Installation 

## Schritt 1: Optional: View settings of cni before installing multus 

```
kubectl debug -it node/worker1 --image=alpine 
```

```
# in der bash
apk add jq
cat /host/etc/cni/net.d/10-calico.conflist | jq
```

```
# Ausgabe

/ # cat /host/etc/cni/net.d/10-calico.conflist | jq
{
  "name": "k8s-pod-network",
  "cniVersion": "0.3.1",
  "plugins": [
    {
      "container_settings": {
        "allow_ip_forwarding": false
      },
      "datastore_type": "kubernetes",
      "endpoint_status_dir": "/var/run/calico/endpoint-status",
      "ipam": {
        "assign_ipv4": "true",
        "assign_ipv6": "false",
        "type": "calico-ipam"
      },
      "kubernetes": {
        "k8s_api_root": "https://10.96.0.1:443",
        "kubeconfig": "/etc/cni/net.d/calico-kubeconfig"
      },
      "log_file_max_age": 30,
      "log_file_max_count": 10,
      "log_file_max_size": 100,
      "log_file_path": "/var/log/calico/cni/cni.log",
      "log_level": "Info",
      "mtu": 0,
      "nodename_file_optional": false,
      "policy": {
        "type": "k8s"
      },
      "policy_setup_timeout_seconds": 0,
      "type": "calico"
    },
    {
      "capabilities": {
        "bandwidth": true
      },
      "type": "bandwidth"
    },
    {
      "capabilities": {
        "portMappings": true
      },
      "snat": true,
      "type": "portmap"
    }
  ]
}
```

## Schritt 2: Installation mit Helm 

```
kubectl apply -f https://raw.githubusercontent.com/k8snetworkplumbingwg/multus-cni/master/deployments/multus-daemonset-thick.yml
```

```
# Überprüfung
kubectl debug -it node/worker1 --image=alpine 
```

```
# in der bash
apk add jq

# das wird als erstes geladen 
cat /host/etc/cni/net.d/00-multus.conf | jq
```


