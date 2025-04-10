# What does policy_setup_timeout_seconds = 0 do ?

## Explanation 

In the Calico CNI (Container Network Interface) configuration, the setting:

```json
"policy_setup_timeout_seconds": 0
```

refers to the **timeout period (in seconds)** that Calico waits for network policy setup to complete during pod creation.

### Here's what it specifically means:

- **`policy_setup_timeout_seconds`** is a configuration parameter used in Calico's CNI plugin to **control how long the plugin will wait for policy enforcement components to be ready** before allowing the pod networking to proceed.
  
- When it's set to `0`, it **disables the wait** entirely. In other words, **Calico will not wait** for policy enforcement to be fully ready before returning control to Kubernetes.

### Why would you use `0`?

- **Faster Pod Startup:** This can help speed up pod startup time, especially in scenarios where you don't want pod creation to be blocked by policy setup.
- **Risk Tradeoff:** You might sacrifice some initial enforcement of network policies — the pod might be up and running **before the policies are applied**.
  
### Typical Use Case:

You might set this to `0`:
- In test environments where speed is prioritized over strict policy enforcement.
- If you have other mechanisms ensuring policies are consistently applied.
- To avoid delays during cluster bootstrap or upgrades.

### TL;DR:

Setting `"policy_setup_timeout_seconds": 0` in the Calico CNI config tells Calico:  
🚫 "Don't wait for policies to be set up before finishing pod networking — just go ahead immediately."

Let me know if you want to explore when it might be risky to use this or best practices around it.
