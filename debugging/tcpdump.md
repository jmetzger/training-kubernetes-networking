# Debugging 

## Prerequisites: Project abi is up and running 



## Debug traffic to pod  

```
# IP des pod apple-app rausfiltern
kubectl get pods -o wide

kubectl debug apple-app -it --image nicolaka/netshoot
# Show processes of other container first 
kubectl debug apple-app -it --image nicolaka/netshoot --target=apple-app
```
### in pod
```
ps aux
tcpdump -n port 5678 
```
 
### in 2. Session (kubectl)

```
kubectl run -it --rm podtester --image=busybox
```

```
wget -O - <ip-des-apple-pods>:5678
```

## Debug traffic to ingress controller 

### mit netshoot connecten

#### Variante 1: Direkt 

```
kubectl -n ingress debug nginx-ingress-ingress-nginx-controller-7bc7c7776d-jpj5h -it --image nicolaka/netshoot
```

```
# in der shell
tcpdump -n port 80
# write to file in pcap format
Older versions of tcpdump truncate packets to 68 or 96 bytes. If this is the case, use -s to capture full-sized packets:
tcpdump -i <interface> -s 65535 -w <file>
```

#### Variante 2: Im Hintergrund laufen lassen und connecten 

```
kubectl -n ingress debug nginx-ingress-ingress-nginx-controller-7bc7c7776d-jpj5h --image nicolaka/netshoot -- sleep infinite
kubectl -n ingress exec -it nginx-ingress-ingress-nginx-controller-7bc7c7776d-jpj5h -c debugger-gwvsr -- zsh
```

```
# in der shell
tcpdump -n port 80
# write to file in pcap format
Older versions of tcpdump truncate packets to 68 or 96 bytes. If this is the case, use -s to capture full-sized packets:
tcpdump -i <interface> -s 65535 -w <file>
```


### Testen

```
# Im browser url aufrufen
# z.B.
http://jochen.lab1.t3isp.de 
```
