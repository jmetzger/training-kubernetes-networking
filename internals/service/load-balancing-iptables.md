# How are iptables set up for load balancing of services && Can i see these rules 

## Explanation 

When you create a Kubernetes **Service of type ClusterIP (default)** that targets 3 backend Pods, Kubernetes uses `iptables` (or `ipvs` depending on kube-proxy mode) to implement load balancing and service routing.

Assuming kube-proxy is running in **iptables mode**, here’s what happens and what kind of `iptables` rules are created:

---

### 🔧 Scenario Overview:

- You have a Service (e.g., `my-service`) with a ClusterIP (e.g., `10.96.0.100`) and port (e.g., `80`)
- The Service selects 3 Pods (let’s say on ports 80) via label selectors
- Kube-proxy on each node programs `iptables` rules

---

### 🔁 What iptables does:

#### 1. **Creates chains in the `nat` table**
Some relevant chains created by kube-proxy:

- `KUBE-SERVICES` – master chain for all services
- `KUBE-SVC-xxxxxxx` – one per service
- `KUBE-SEP-xxxxxxx` – one per endpoint (Pod IP)

#### 2. **Matching packets to service IP**
The rule in `KUBE-SERVICES` looks like:

```bash
-A KUBE-SERVICES -d 10.96.0.100/32 --dport 80 -j KUBE-SVC-XXXXXXX
```

#### 3. **Load balancing via `KUBE-SVC-XXXXXXX`**
This chain chooses a backend via random probability matching:

```bash
-A KUBE-SVC-XXXXXXX -m statistic --mode random --probability 0.3333 -j KUBE-SEP-AAAAAAA
-A KUBE-SVC-XXXXXXX -m statistic --mode random --probability 0.5000 -j KUBE-SEP-BBBBBBB
-A KUBE-SVC-XXXXXXX -j KUBE-SEP-CCCCCCC
```

> These rules distribute traffic roughly evenly to each of the 3 Pods.

#### 4. **Endpoint chains DNAT to Pod IPs**
Each endpoint chain contains a DNAT rule:

```bash
-A KUBE-SEP-AAAAAAA -s <client CIDRs> -j DNAT --to-destination 10.244.0.5:8080
-A KUBE-SEP-BBBBBBB -s <client CIDRs> -j DNAT --to-destination 10.244.1.3:8080
-A KUBE-SEP-CCCCCCC -s <client CIDRs> -j DNAT --to-destination 10.244.2.7:8080
```

---

### 🔍 To inspect this yourself:

```bash
sudo iptables -t nat -L -n --line-numbers
```

Or narrow it down:

```bash
sudo iptables -t nat -S | grep KUBE
```

You’ll see the full chain of logic from service IP to actual Pod IPs via `DNAT`.

---

### ⚠️ Notes:

- This only applies if kube-proxy is in **iptables mode** (you can check with `kubectl get cm kube-proxy -n kube-system -o yaml`)
- If it's using **IPVS mode**, the logic is similar but handled via the `ipvsadm` tool
- kube-proxy updates these rules dynamically as Pods are added/removed


## Setup for testing 

```
cd
mkdir -p manifests/service-iptables/
cd manifests/service-iptables/
nano 01-service-deploy.yml
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx-demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: nginx-demo
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80       # Service port
    targetPort: 80 # Pod port
  type: ClusterIP
```

```
kubectl apply -f .
```
