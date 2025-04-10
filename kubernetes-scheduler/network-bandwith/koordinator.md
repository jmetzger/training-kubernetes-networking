# Is there a scheduler, that can take Network Bandwith into account ?

## Explanation 

To configure **Koordinator** for **network-aware scheduling** in your Kubernetes cluster, you can follow these steps:

---

### 1. **Install Koordinator**

Ensure that Koordinator is installed in your cluster. If it's not already installed, you can add the Koordinator Helm repository and install it using Helm:


```bash
helm repo add koordinator https://koordinator-sh.github.io/charts/
helm repo update
helm install koordinator koordinator/koordinator --namespace koordinator-system --create-namespace
```


---

### 2. **Enable Load-Aware Scheduling**

Koordinator's **Load-Aware Scheduling** feature allows the scheduler to consider real-time node metrics, including CPU and memory usage, to make informed scheduling decisions. This feature is enabled by default. However, you can customize its behavior by modifying the `koord-scheduler-config` ConfigMap.

To customize the configuration:

- Edit the `koord-scheduler-config` ConfigMap:

  
```bash
  kubectl edit configmap koord-scheduler-config -n koordinator-system
  ```


- In the `data.koord-scheduler-config` section, you can configure thresholds and weights for resources, influencing how the scheduler balances loads across nodes. For example:

  
```yaml
  apiVersion: kubescheduler.config.k8s.io/v1beta2
  kind: KubeSchedulerConfiguration
  profiles:
    - schedulerName: koord-scheduler
      plugins:
        filter:
          enabled:
            - name: LoadAwareScheduling
        score:
          enabled:
            - name: LoadAwareScheduling
              weight: 1
        reserve:
          enabled:
            - name: LoadAwareScheduling
      pluginConfig:
        - name: LoadAwareScheduling
          args:
            # Configure thresholds and weights here
  ```


  This configuration enables the `LoadAwareScheduling` plugin and allows you to set specific thresholds and weights. For more detailed information on configuring these parameters, refer to the [Koordinator Load-Aware Scheduling Documentation](https://koordinator.sh/docs/user-manuals/load-aware-scheduling/).

---

### 3. **Annotate Nodes with Custom Usage Thresholds (Optional)**

For more granular control, you can set specific load thresholds on individual nodes using annotations. By adding the `scheduling.koordinator.sh/usage-thresholds` annotation to a node, you can define custom utilization thresholds that Koordinator will consider during scheduling.

For example, to set a CPU usage threshold of 80% and a memory usage threshold of 70% on a node:


```bash
kubectl annotate node <node-name> scheduling.koordinator.sh/usage-thresholds='{"usageThresholds":{"cpu":80,"memory":70}}'
```


Replace `<node-name>` with the name of your node. This annotation helps Koordinator make more informed scheduling decisions based on the real-time load of each node. Detailed instructions can be found in the [Koordinator Load-Aware Scheduling Documentation](https://koordinator.sh/docs/user-manuals/load-aware-scheduling/).

---

### 4. **Deploy Pods Using Koordinator's Scheduler**

When deploying your applications, specify `koord-scheduler` as the scheduler in your Pod or Deployment manifests to utilize Koordinator's scheduling capabilities.

Here's an example of a Deployment manifest:


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      schedulerName: koord-scheduler
      containers:
        - name: example-container
          image: example-image
          ports:
            - containerPort: 80
```

In this manifest:

- The `schedulerName: koord-scheduler` line directs Kubernetes to use Koordinator's scheduler for this Deployment.
- The rest of the manifest defines a typical Deployment with three replicas of a container named `example-container`.

By setting the `schedulerName` to `koord-scheduler`, Koordinator will handle the scheduling of these Pods, taking into account network traffic and other factors to optimize performance. For more information on configuring multiple schedulers, refer to the [Kubernetes Documentation](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/).
