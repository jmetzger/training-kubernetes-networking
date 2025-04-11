# Mixed Routing-Mode BGP / vxlancrossnet

## Mode: 

  * vxlancrossnet

## What does it do ? 

  * BGP is used, when other node is directly reachable (no restriction through firewalling / networking)
  * vxlan is used, when worker node is not directly reachable (using BGP)

## Grafics

![image](https://github.com/jmetzger/training-kubernetes-networking/assets/1933318/a2766737-e1e5-4ee0-8e03-9216a0379d97)


## Detailed Explanation 

Klar! Lass uns gemeinsam **`vxlancrossnet` bei Calico** anschauen:  
Ich erkläre erst kurz, was es ist, warum es wichtig ist, und zeige dir dann ein Beispiel mit einer passenden Infografik.

---

### 🔍 Was ist `vxlancrossnet` in Calico?

`vxlancrossnet` ist ein **Overlay-Modus von Calico**, der es ermöglicht, Pods auf verschiedenen Layer-3-Netzwerken (z. B. in getrennten Subnetzen oder Clouds) miteinander zu verbinden – **selbst wenn es keine direkte IP-Routen zwischen den Nodes gibt**.

Es funktioniert, indem **VXLAN-Tunnel** aufgebaut werden, um Pod-Verkehr zu kapseln und übers Netzwerk zu transportieren.

---

### 🧠 Warum braucht man `vxlancrossnet`?

Standardmäßig funktioniert Calico im **BGP-Modus oder mit IP-IP/VXLAN Overlays**, wenn die Nodes sich gegenseitig routen können.

Aber:

- In **Cloud-Umgebungen**, wo Nodes in **verschiedenen VPCs/Subnetzen** liegen,
- oder in hybriden Szenarien (on-prem + cloud),
- ist manchmal **kein direktes Routing** zwischen den Nodes möglich.

👉 `vxlancrossnet` löst das, indem es Pods trotzdem über ein **VXLAN-Overlay** miteinander kommunizieren lässt.

---

### 🔧 Beispiel: Cluster mit zwei Nodes in getrennten Subnetzen

**Scenario**:

- Node A: `10.0.0.10` (Subnetz 10.0.0.0/24)
- Node B: `10.1.0.20` (Subnetz 10.1.0.0/24)
- Kein Routing zwischen den Subnetzen möglich (Security Groups, Firewalls, etc.)
- Ziel: Pod auf Node A soll mit Pod auf Node B kommunizieren

👉 Mit `vxlancrossnet` kapselt Calico den Pod-Traffic in ein VXLAN-Paket und überträgt es zwischen den Nodes – auch ohne Routing zwischen den Subnetzen.

---

### 📦 Beispielhafte Konfiguration

Du kannst `vxlancrossnet` in Calico so aktivieren:

```yaml
apiVersion: crd.projectcalico.org/v1
kind: IPPool
metadata:
  name: default-pool
spec:
  cidr: 192.168.0.0/16
  vxlanMode: CrossSubnet  # oder Always
  encapsulation: VXLAN
  blockSize: 26
  natOutgoing: true
  nodeSelector: all()
```

---

## How to find out, if this node is used 

```
kubectl -n calico-system get ippool -o yaml | grep vxlan 
```
