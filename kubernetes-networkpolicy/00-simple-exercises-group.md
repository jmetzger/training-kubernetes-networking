# Simple Network Policy Example 

```
# Hier bitte Euer Kürzel eintragen 
KURZ=jm
```

```
# Schritt 1:
kubectl create ns policy-demo-<name-kurz>
kubectl create deployment --namespace=policy-demo-$KURZ nginx --image=nginx:1.21
kubectl expose --namespace=policy-demo-$KURZ deployment nginx --port=80
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
```
```
# innerhalb der shell 
wget -q nginx -O -
```
```
# Schritt 2: Policy festlegen, dass kein Ingress-Traffic erlaubt
# in diesem namespace: policy-demo-$KURZ 
kubectl create -f - <<EOF
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: default-deny
  namespace: policy-demo-$KURZ
spec:
  podSelector:
    matchLabels: {}
EOF
# lassen einen 2. pod laufen mit dem auf den nginx zugreifen 
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
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
  namespace: policy-demo-$KURZ
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
kubectl run --namespace=policy-demo-$KURZ access --rm -ti --image busybox
```

```
# innerhalb der shell 
wget -q nginx -O -
```

``` 
kubectl run --namespace=policy-demo-$KURZ no-access --rm -ti --image busybox
```

```
# in der shell  
wget -q nginx -O -
```

```

kubectl delete ns policy-demo-$KURZ 

```


## Ref:

  * https://projectcalico.docs.tigera.io/security/tutorials/kubernetes-policy-basic