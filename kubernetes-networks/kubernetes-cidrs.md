# Kubernetes CIDRs 

## Grafik 

![image](https://github.com/user-attachments/assets/87bb7926-d962-4f71-b8b4-f04b7ab44ec6)

## Cluster CIDR - IP-Bereich für das gesamte Kubernetes Cluster 

```
# Netzbereich für mein gesamtes Cluster 
10.244.0.0/16
```

## POD-CIDR - Teilbereich aus der Cluster - CIDR pro Node 

```
# Jede Node bekommt ein Teilnetz
Beispiel cilium

node 1 -> network.cilium.io/ipv4-pod-cidr: 10.244.0.0/25
node 2 -> network.cilium.io/ipv4-pod-cidr: 10.244.0.128/25
node 3 -> network.cilium.io/ipv4-pod-cidr: 10.244.1.128/25
node 4 -> network.cilium.io/ipv4-pod-cidr: 10.244.1.0/25

```

## POD-IP 

  * Wird aus POD-CIDR des jeweiligen Nodes vergeben

```
# pod bekommt aus netzbereich POD-CIDR auf Node eine IP-Adresse zugewiesen
# CILIUM CNI macht das z.B.
POD-CIDR: 10.244.1.128/25
-> POD - IP: 10.244.1.180
```

## Service-CIDR 

```
Netzbereich für IP-Adressen der Services
z.B. 10.109.0.0/16 
```
