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

Let me know if you want help checking what MTU Calico is actually using on your nodes.
