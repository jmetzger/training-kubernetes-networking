# Using different cluster-cidr with kubeadm 

## Voraussetzungen: 

  * Alle Nodes sind bereits mit os (z.B. Ubuntu 24.04.) und installierten Kernkomponenten versehen

```
kubelet
kubeadm
containerd
```

## Schritt 1: Nur auf control plane notwendig (d.h. init) 

```
# Initalisiert das control plane mit anderer cluster cidr 
kubeadm init --pod-network-cidr=10.178.0.0/16

```

## Schritt 2: weitere Cluster Nodes ausrollen und joinen 

```
....
```

## Schritt 3: Installation Object für calico anders bauen 

```
# This section includes base Calico installation configuration.
# For more information, see: https://docs.tigera.io/calico/latest/reference/installation/api#operator.tigera.io/v1.Installation
apiVersion: operator.tigera.io/v1
kind: Installation
metadata:
  name: default
spec:
  # Configures Calico networking.
  calicoNetwork:
    ipPools:
    - name: default-ipv4-ippool
      blockSize: 26
      cidr: 10.178.0.0/16
      encapsulation: VXLANCrossSubnet
      natOutgoing: Enabled
      nodeSelector: all()

---

# This section configures the Calico API server.
# For more information, see: https://docs.tigera.io/calico/latest/reference/installation/api#operator.tigera.io/v1.APIServer
apiVersion: operator.tigera.io/v1
kind: APIServer
metadata:
  name: default
spec: {}

---

```
