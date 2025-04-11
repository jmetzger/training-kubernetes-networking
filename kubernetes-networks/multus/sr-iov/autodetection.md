# sr-iov - Autodetection 

Yes, it’s technically possible to **autodetect SR-IOV NICs** on a Kubernetes node and dynamically configure a **Multus `NetworkAttachmentDefinition`** to attach a pod to a randomly selected one — **but not entirely natively** or dynamically at runtime without help.

### Let's break it down:

---

### 🔍 SR-IOV Interface Detection

You can detect all SR-IOV capable NICs on a node using tools like:

```bash
lspci | grep -i ether
```

Or more specifically:

```bash
ls /sys/class/net | while read iface; do
  if [ -d "/sys/class/net/$iface/device/sriov_totalvfs" ]; then
    echo "$iface is SR-IOV capable"
  fi
done
```

The `sriov-device-plugin` used with Kubernetes typically discovers and advertises these VFs to the Kubelet.

---

### 📦 Multus + SR-IOV

Multus uses `NetworkAttachmentDefinition` (NAD) with SR-IOV by referencing a resource (like `intel.com/sriov_netdevice`). This ties into the `sriov-network-device-plugin`, which assigns Virtual Functions (VFs) to pods based on availability.

But **you can't directly say "randomly pick an SR-IOV interface" in the NAD JSON**. It needs a defined resource name and configuration — unless you automate generating NADs.

---

### 🛠️ So how do you make this dynamic?

#### Option 1: Dynamic NAD Generator (Custom Controller or Admission Webhook)
- Create a custom mutating webhook or initContainer that:
  - Detects all available SR-IOV devices on the node.
  - Randomly picks one.
  - Generates a NAD dynamically (with unique name) or patches the pod spec to refer to the chosen one.

#### Option 2: Predefine Multiple NADs
- Define multiple NADs manually, each bound to a specific SR-IOV interface.
- In your app logic or scheduling controller, randomly select one when creating the pod.

#### Option 3: Use SR-IOV device plugin with resource pool
- Let the `sriov-network-operator` manage pools (e.g., via `SriovNetworkNodePolicy`) and abstract the devices.
- Pods just request a resource (e.g., `intel.com/sriov_netdevice`) and the device plugin picks from the available pool.

This is not *truly random per pod*, but it’s **automatically distributed** if you're okay with that.

---

### ✅ TL;DR

- **Auto-detecting SR-IOV NICs** = easy via sysfs or `sriov-network-device-plugin`.
- **Random assignment in Multus NAD** = not directly supported.
- **Solutions**:
  - Use device plugin + pool (simpler).
  - Generate NADs dynamically via custom logic (more flexible).

---

Would you prefer **random selection per pod** at runtime, or would **even distribution** via device plugin be enough? Depending on your goal, I can help you script either.
