# Install calico-api-server 

## prepare kube-api-server for to be use for calico calls. 

  * Possible from calico 3.20+ (GA) 
  * https://docs.tigera.io/calico/latest/operations/install-apiserver


### Step 1: Apply manifests for api server 

```
cd
mkdir -p manifests 
cd manifests 
# calico api server 
mkdir cas 
cd cas 
vi cas.yaml 
```


```
# taken from https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/apiserver.yaml
# but adjusted images version to corresponding installation 
# kubectl -n kube-system get ds calico-node -o=jsonpath='{.spec.template.spec.containers[0].image}'
# This is a tech-preview manifest which installs the Calico API server. Note that this manifest is liable to change
# or be removed in future releases without further warning.
#
# Namespace and namespace-scoped resources.
apiVersion: v1
kind: Namespace
metadata:
  labels:
    name: calico-apiserver
  name: calico-apiserver
spec:

---

# Policy to ensure the API server isn't cut off. Can be modified, but ensure 
# that the main API server is always able to reach the Calico API server.
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: allow-apiserver
  namespace: calico-apiserver
spec:
  podSelector:
    matchLabels:
      apiserver: "true"
  ingress:
  - ports:
    - protocol: TCP
      port: 5443

---

apiVersion: v1
kind: Service
metadata:
  name: calico-api
  namespace: calico-apiserver
spec:
  ports:
  - name: apiserver
    port: 443
    protocol: TCP
    targetPort: 5443
  selector:
    apiserver: "true"
  type: ClusterIP

---

apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    apiserver: "true"
    k8s-app: calico-apiserver
  name: calico-apiserver
  namespace: calico-apiserver
spec:
  replicas: 1
  selector:
    matchLabels:
      apiserver: "true"
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        apiserver: "true"
        k8s-app: calico-apiserver
      name: calico-apiserver
      namespace: calico-apiserver
    spec:
      containers:
      - args:
        - --secure-port=5443
        # - -v=5 # not working in v3.23.5 not available as flag there 
        env:
        - name: DATASTORE_TYPE
          value: kubernetes
        image: calico/apiserver:v3.23.5
        livenessProbe:
          httpGet:
            path: /version
            port: 5443
            scheme: HTTPS
          initialDelaySeconds: 90
          periodSeconds: 10
        name: calico-apiserver
        readinessProbe:
          exec:
            command:
            - /code/filecheck
          failureThreshold: 5
          initialDelaySeconds: 5
          periodSeconds: 10
        securityContext:
          privileged: false
          runAsUser: 0
        volumeMounts:
        - mountPath: /code/apiserver.local.config/certificates
          name: calico-apiserver-certs
      dnsPolicy: ClusterFirst
      nodeSelector:
        kubernetes.io/os: linux
      restartPolicy: Always
      serviceAccount: calico-apiserver
      serviceAccountName: calico-apiserver
      tolerations:
      - effect: NoSchedule
        key: node-role.kubernetes.io/master
      - effect: NoSchedule
        key: node-role.kubernetes.io/control-plane
      volumes:
      - name: calico-apiserver-certs
        secret:
          secretName: calico-apiserver-certs

---

apiVersion: v1
kind: ServiceAccount
metadata:
  name: calico-apiserver 
  namespace: calico-apiserver 

---

# Cluster-scoped resources below here.
apiVersion: apiregistration.k8s.io/v1
kind: APIService
metadata:
  name: v3.projectcalico.org
spec:
  group: projectcalico.org
  groupPriorityMinimum: 1500
  service:
    name: calico-api
    namespace: calico-apiserver
    port: 443
  version: v3
  versionPriority: 200

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: calico-crds
rules:
- apiGroups:
  - extensions
  - networking.k8s.io
  - ""
  resources:
  - networkpolicies
  - nodes
  - namespaces
  - pods
  - serviceaccounts
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - crd.projectcalico.org
  resources:
  - globalnetworkpolicies
  - networkpolicies
  - clusterinformations
  - hostendpoints
  - globalnetworksets
  - networksets
  - bgpconfigurations
  - bgppeers
  - felixconfigurations
  - kubecontrollersconfigurations
  - ippools
  - ipreservations
  - ipamblocks
  - blockaffinities
  - caliconodestatuses
  - ipamconfigs
  verbs:
  - get
  - list
  - watch
  - create
  - update
  - delete
- apiGroups:
  - policy
  resourceNames:
  - calico-apiserver
  resources:
  - podsecuritypolicies
  verbs:
  - use

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: calico-extension-apiserver-auth-access
rules:
- apiGroups:
  - ""
  resourceNames:
  - extension-apiserver-authentication
  resources:
  - configmaps
  verbs:
  - list
  - watch
  - get
- apiGroups:
  - rbac.authorization.k8s.io
  resources:
  - clusterroles
  - clusterrolebindings
  - roles
  - rolebindings
  verbs:
  - get
  - list
  - watch

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: calico-webhook-reader
rules:
- apiGroups:
  - admissionregistration.k8s.io
  resources:
  - mutatingwebhookconfigurations
  - validatingwebhookconfigurations
  verbs:
  - get
  - list
  - watch

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calico-apiserver-access-crds
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: calico-crds
subjects:
- kind: ServiceAccount
  name: calico-apiserver
  namespace: calico-apiserver

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calico-apiserver-delegate-auth
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: system:auth-delegator
subjects:
- kind: ServiceAccount
  name: calico-apiserver
  namespace: calico-apiserver

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calico-apiserver-webhook-reader
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: calico-webhook-reader
subjects:
- kind: ServiceAccount
  name: calico-apiserver
  namespace: calico-apiserver

---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: calico-extension-apiserver-auth-access
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: calico-extension-apiserver-auth-access
subjects:
- kind: ServiceAccount
  name: calico-apiserver
  namespace: calico-apiserver

```

### Step 2: create certificates 

```
openssl req -x509 -nodes -newkey rsa:4096 -keyout apiserver.key -out apiserver.crt -days 365 -subj "/" -addext "subjectAltName = DNS:calico-api.calico-apiserver.svc"
kubectl create secret -n calico-apiserver generic calico-apiserver-certs --from-file=apiserver.key --from-file=apiserver.crt
# configure server with ca-bundle 
kubectl patch apiservice v3.projectcalico.org -p \
    "{\"spec\": {\"caBundle\": \"$(kubectl get secret -n calico-apiserver calico-apiserver-certs -o go-template='{{ index .data "apiserver.crt" }}')\"}}"

```

## Step 3: check if it is working 

```
kubectl api-resources | grep '\sprojectcalico.org'
# only available in v3 
kubectl get clusterinfo 
```
