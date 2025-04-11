# What is mtu=0 in cni config of calico 

## Explanation 

In the **CNI (Container Network Interface) config for Calico**, the `"mtu": 0` setting might look confusing at first. Here's what it means:

---

### 🚀 What does `"mtu": 0` mean?

Setting `"mtu": 0` **tells Calico to automatically detect the MTU** (Maximum Transmission Unit) based on the underlying network interface.

---

### 🧠 Why would you do that?

Instead of hardcoding a specific MTU (like 1500 or 1450), using `0` delegates the responsibility to Calico's CNI plugin, which will:

- Discover the MTU of the primary network interface on the node.
- Subtract necessary overhead (like VXLAN, WireGuard, IP-in-IP headers, etc., depending on your encapsulation mode).
- Use the appropriate MTU for the workload interfaces to avoid fragmentation issues.

---

### 📌 Example Use Case

In a hybrid cluster with nodes running on different types of networks (cloud, on-prem), hardcoding MTU might lead to:

- **Packet fragmentation**
- **Network errors**
- **Unpredictable performance**

By using `"mtu": 0`, Calico adapts automatically.

---

### ✅ Summary

| Key | Value |
|-----|-------|
| `"mtu": 0` | Auto-detect MTU based on host interface |
| Good for | Avoiding manual config, supporting diverse environments |
| When to use | Most of the time unless you have a specific MTU requirement |

---

Alright! Here's how you can **check what MTU Calico is actually using** on your nodes:

---

### 🛠️ Option 1: Inspect a Calico CNI-created interface inside a pod

Run a pod and check the interface MTU inside it:

```bash
kubectl run -it --rm test-pod --image=alpine -- sh
```

Then inside the pod:

```sh
ip link
```

Look for the `eth0` interface — it’s usually the one used for communication — and you’ll see something like:

```
3: eth0@if5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1440 ...
```

✅ This MTU (`1440` here) is what Calico assigned to the pod's virtual interface, based on the host MTU and overhead.

---

### 🛠️ Option 2: Check on the host what MTU Calico chose

SSH into one of your nodes and look for Calico interfaces:

```bash
ip link | grep cali
```

You might see:

```
15: cali12345678@if16: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1440 ...
```

That shows the MTU used for the host-side veth of the pod.

---

### 🛠️ Option 3: Look at the actual auto-detected MTU logic in the logs

You can grep the logs of the Calico CNI plugin or Felix:

```bash
kubectl -n kube-system logs -l k8s-app=calico-node | grep -i mtu
```

Or on some setups:

```bash
kubectl -n kube-system logs daemonset/calico-node | grep -i mtu
```

You may see lines like:

```
Auto-detected interface eth0 with MTU 1500
Calico CNI MTU set to 1440 (1500 - 60)
```

That confirms both the detected MTU and how it subtracts encapsulation overhead.

---

If you'd like, I can help you write a little automated script to grab MTU values across all nodes or pods. Want that?
