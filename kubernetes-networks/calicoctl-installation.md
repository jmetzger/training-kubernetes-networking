# Calico 

## calicoctl Installation walkthrough 

### Version rausbekommen 

```
# welche version von calico setzen wir aktuell auf dem server ein 
kubectl -n kube-system get ds calico-node -o=jsonpath='{.spec.template.spec.containers[0].image}'
# docker.io/calico/node:v3.23.5

```

### Pod erstellen für calicoctl auf Basis von 

```
mkdir -p manifests
cd manifests 
mkdir calicoctl 
cd calicoctl 
vi calicoctl.yaml
```

```
## https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calicoctl.yaml
## Calico Version master
# https://projectcalico.docs.tigera.io/releases#master
# This manifest includes the following component versions:
#   calico/ctl:v3.25.1



apiVersion: v1
kind: ServiceAccount
metadata:
  name: calicoctl
  namespace: kube-system

---

apiVersion: v1
kind: Pod
metadata:
  name: calicoctl
  namespace: kube-system
spec:
  nodeSelector:
    kubernetes.io/os: linux
  hostNetwork: true
  serviceAccountName: calicoctl
  containers:
  - name: calicoctl
    image: calico/ctl:v3.23.5
    command:
      - /calicoctl
    args:
      - version
      - --poll=1m
    env:
    - name: DATASTORE_TYPE
      value: kubernetes

---

kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: calicoctl
rules:
  - apiGroups: [""]
    resources:
      - namespaces
      - nodes
    verbs:
      - get
      - list
      - update
  - apiGroups: [""]
    resources:
      - nodes/status
    verbs:
      - update
  - apiGroups: [""]
    resources:
      - pods
      - serviceaccounts
    verbs:
      - get
      - list
  - apiGroups: [""]
    resources:
      - pods/status
    verbs:
      - update
  - apiGroups: ["crd.projectcalico.org"]
    resources:
      - bgppeers
      - bgpconfigurations
      - clusterinformations
      - felixconfigurations
      - globalnetworkpolicies
      - globalnetworksets
      - ippools
      - ipreservations
      - kubecontrollersconfigurations
      - networkpolicies
      - networksets
      - hostendpoints
      - ipamblocks
      - blockaffinities
      - ipamhandles
      - ipamconfigs
    verbs:
      - create
      - get
      - list
      - update
      - delete
  - apiGroups: ["networking.k8s.io"]
    resources:
      - networkpolicies
    verbs:
      - get
      - list

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calicoctl
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: calicoctl
subjects:
- kind: ServiceAccount
  name: calicoctl
  namespace: kube-system
  
```
