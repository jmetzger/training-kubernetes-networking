# Exercise way of a packet 

## Schritt 1: Vorbereitung:

```
# Achtung: Nodes anpassen
NODE1=telekom-xwkb3
NODE2=telekom-xwkb8
```

```
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: curl
  name: curl
spec:
  containers:
  - image: curlimages/curl
    name: curl
    command: ["sleep", "365d"]
  nodeName: $NODE1
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: httpbin
  name: httpbin
spec:
  containers:
  - image: kennethreitz/httpbin
    name: httpbin
  nodeName: $NODE2
EOF

```

## Schritt 1.5: Wir ziehen die Namen der cilium-agents für die zugehörigen Nodes 

  * Und speichernn diese als Variablen 

```
cilium1=$(kubectl get po -n kube-system -l k8s-app=cilium --field-selector spec.nodeName=$NODE1 -o jsonpath='{.items[0].metadata.name}')
cilium2=$(kubectl get po -n kube-system -l k8s-app=cilium --field-selector spec.nodeName=$NODE2 -o jsonpath='{.items[0].metadata.name}')
```

## Schritt 2: Pod-IP des Ziels ausfindig machen und aufrufen. 

```
kubectl get po httpbin -o wide
```

```
# Beispielausgabe
10.244.1.246
```

## Schritt 3: Zielsystem from Anfrager ausrufen (curl-pod) 

```
# Verbindung funktioniert 
kubectl exec -it curl -- curl http://10.244.1.246:80/
```

## Schritt 3.1: CURL Debuggen 1: Lass uns die Route zu 10.244.1.246 herausfinden (von curl aus) 

```
kubectl exec curl -- ip route get 10.244.1.246
```

```
# Beispielausgabe
10.244.1.246 via 10.244.0.237 dev eth0  src 10.244.0.153
```

## Schritt 3.2. CURL: Debuggen 2: Wie ist die Mac-Adresse von 10.244.0.237 

  * **Wichtig:** Wir wollen über eth0 des containers rausgehen.

```
# Lasst und die Mac der IP herausfinden
kubectl exec curl  -- arp -n
```

```
# Beispielausgabe
? (10.244.0.237) at f6:25:f2:9e:91:49 [ether]  on eth0
```

## Schritt 3.3 CURL Debuggen 3: Ist das die Mac-Adresse des eth0 interfaces im Container ? 

```
# Herausgefunden haben wir als Beispiel
f6:25:f2:9e:91:49 [ether]  on eth0
```

```
# Das ist aber die MAC des eth0 - Adapter im pod (curl) 
kubectl exec curl -- ip link show eth0
```

```
# Ausgabe:
256: eth0@if257: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP qlen 1000
    link/ether 9a:b4:aa:83:18:62 brd ff:ff:ff:ff:ff:ff
```

  * **ACHTUNG** - andere MAC und Teil eines veth - Paares (eth0@if257)- der andere Teil ist das Interface 257 auf dem Host (Worker Node) 

## Schritt 3.4 Das Interface auf dem Host-System finden 

```
kubectl debug --rm -it node/$NODE1 --image=busybox -- sh
```

```
# in der shell nr bitte anpassen
ip link | grep -A1 ^257 
```

```
# Beispielausgabe # OK !!! gleiche MAC wie aus Schritt 3.3. 
257: lxced3096cb2cb1@if256: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue qlen 1000
    link/ether f6:25:f2:9e:91:49 brd ff:ff:ff:ff:ff:ff
```

## Schritt 3.5 Statt der Bridge, was ? 

  * Bei cilium wird keine Bridge verwendet, sondern der Weg geht über eBPF (barkeley package filter programme in der Kernel geladen)

```
# Wir wollen jetzt rausfinden, welche Regeln dort greift, dazu Fragen wir den cilium - Agenten auf Node 1
# den wir in der Variablen cilium1 gesetzt haben 
# und wir brauchen das interface aus 3.4.
# ohne @
lxced3096cb2cb1
```

```
# Bemerkung:
# Since the container's eth0 and the host network namespace's lxc constitute a channel, the container's egress (Egress) traffic is the lxc ingress Ingress traffic. Similarly, the container's ingress traffic is the lxc egress traffic.
# Wir fragen also nach Ingress - Regeln
```

```
kubectl exec -n kube-system $cilium1 -c cilium-agent -- bpftool net show dev lxced3096cb2cb1
```

```
# Beispielausgabe:
# es gibt also eine regel die an tc gebunden ist für ingress 
xdp:

tc:
lxced3096cb2cb1(257) clsact/ingress cil_from_container-lxced3096cb2cb1 id 2490

flow_dissector:
```



