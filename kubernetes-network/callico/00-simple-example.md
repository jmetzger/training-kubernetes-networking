# Simple Example Calico

```
# Schritt 1:
kubectl create ns policy-demo
kubectl create deployment --namespace=policy-demo nginx --image=nginx
kubectl expose --namespace=policy-demo deployment nginx --port=80
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox
```
```
# innerhalb der shell 
wget -q nginx -O -
```
```
# Schritt 2: Policy festlegen, dass kein Ingress-Traffic erlaubt
# in diesem namespace: policy-demo 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny
  namespace: policy-demo
spec:
  podSelector:
    matchLabels: {}
EOF
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```

```
# innerhalb der shell 
wget -q nginx -O -
```

```
# Schritt 3: Zugriff erlauben von pods mit dem Label run=access 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: access-nginx
  namespace: policy-demo
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: access
EOF

# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
# pod hat durch run -> access automatisch das label run:access zugewiesen 
kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
```

```
# innerhalb der shell 
wget -q nginx -O -
```

``` 
kubectl run --namespace=policy-demo no-access --rm -ti --image busybox /bin/sh
```

```
# in der shell  
wget -q nginx -O -
```

```

kubectl delete ns policy-demo 

```


## Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic
