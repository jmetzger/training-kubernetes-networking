# Exercise way of a packet 

## What we want to check ? 

## Schritt 1: Vorbereitung:

```
# Achtung: Nodes anpassen
NODE1=worker1
NODE2=worker2
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

## Schritt 2: Pod-IP des Ziels ausfindig machen und aufrufen. 

```
kubectl get po httpbin -o wide
```

```
# Beispielausgabe
10.244.1.246
```

## Schritt 3: Ziel-Pod (httpbin auf NODE2) von curl auf NODE1 aus aufrufen

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
# Ausgabe: / OH eine ganz andere MAC ! 
256: eth0@if257: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP qlen 1000
    link/ether 9a:b4:aa:83:18:62 brd ff:ff:ff:ff:ff:ff
```

  * **ACHTUNG** - andere MAC und Teil eines veth - Paares (eth0@if257)- der andere Teil ist das Interface 257 auf dem Host (Worker Node) 

## Schritt 3.4 Das Interface auf dem Host-System finden 

```
kubectl debug -it node/$NODE1 --image=busybox -- sh
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

  *  **Calico hijacks ARP (proxy_arp) table of POD1, forces the next hop to be the peer end (host side) of the veth pair.**


```
kubectl get nodes -o wide 
```

## Schritt 4: On Node 2: 



## Reference:

  * https://addozhang.medium.com/kubernetes-network-learning-with-cilium-and-ebpf-aafbf3163840
