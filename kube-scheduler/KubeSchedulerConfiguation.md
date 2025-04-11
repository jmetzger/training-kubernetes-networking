# Configure the kube-scheduler with KubeSchedulerConfiguration 

## Explanation 

The `KubeSchedulerConfiguration` is used to configure the Kubernetes scheduler component. If you're trying to customize the behavior of the kube-scheduler (like configuring profiles, plugins, or leader election settings), you'd typically use this configuration in a file and then launch the scheduler with it.

---

### ✅ How to Apply `KubeSchedulerConfiguration`

1. **Create the Configuration File**
   Here's a basic example:

   ```yaml
   apiVersion: kubescheduler.config.k8s.io/v1
   kind: KubeSchedulerConfiguration
   clientConnection:
     kubeconfig: /etc/kubernetes/scheduler.conf
   leaderElection:
     leaderElect: true
   profiles:
     - schedulerName: default-scheduler
       plugins:
         score:
           enabled:
             - name: NodeResourcesFit
   ```

2. **Start kube-scheduler with the Config File**
   When starting the `kube-scheduler`, specify the configuration file:

   ```bash
   kube-scheduler --config=/path/to/kube-scheduler-config.yaml
   ```

   If you're using a managed Kubernetes service (e.g., GKE, EKS, AKS), you **won’t** have direct access to the scheduler process and can't customize it this way.

---

### 🧐 Why It Might Not Be Available

If you're getting an error like `no matches for kind "KubeSchedulerConfiguration"` or `kubescheduler.config.k8s.io/v1 not recognized`, here are some possible reasons:

#### 1. **Wrong API Version**
Make sure the API version matches your Kubernetes version:
- `v1beta1` for Kubernetes <= 1.22
- `v1` for Kubernetes >= 1.23

Check your cluster version:
```bash
kubectl version --short
```

#### 2. **You're Applying It With `kubectl apply`**
This type is **not a Kubernetes API resource**—you **do not apply it with `kubectl apply`**. It’s a **component config**, not a runtime object. The scheduler reads it at startup.

#### 3. **You Don’t Have Control Over kube-scheduler**
If your cluster is managed (like GKE, EKS, etc.), you can’t change the scheduler config directly. You’d need to run your **own scheduler** if you want to customize it.

---

### 💡 What You Can Do

- If you're running a **self-managed cluster**, modify the scheduler's manifest in `/etc/kubernetes/manifests/kube-scheduler.yaml` (on the control plane node) to point to your custom config.
- If you're using **kubeadm**, you can configure it in the kubeadm config file before initializing the cluster.
- For testing, run the scheduler manually using your config on a test cluster (e.g., with `kind`, `minikube`, or `kubeadm`).

---

Want help writing a config file or modifying an existing scheduler deployment?
