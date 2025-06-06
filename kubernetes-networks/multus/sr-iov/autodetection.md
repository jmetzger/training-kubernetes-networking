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

Perfect — you're aiming for **random selection of a specific SR-IOV VF per pod**, which is a bit outside of what Multus and the SR-IOV device plugin do by default. But with a bit of Kubernetes magic, we can make it work.

Here’s a practical **design pattern** to achieve this:

---

## 🧩 Strategy for Random SR-IOV NIC Selection Per Pod

### 1. **Predefine NADs for Each SR-IOV Interface**
- You create one `NetworkAttachmentDefinition` per physical interface (e.g., `sriov-net-ens1f0`, `sriov-net-ens2f0`, etc.)
- Each NAD refers to a specific `resourceName` exposed by the SR-IOV device plugin.

#### Example NAD for `ens1f0`:
```yaml
apiVersion: k8s.cni.cncf.io/v1
kind: NetworkAttachmentDefinition
metadata:
  name: sriov-net-ens1f0
spec:
  config: '{
    "type": "sriov",
    "name": "sriov-net-ens1f0",
    "ipam": {
      "type": "static"
    },
    "deviceID": "0000:01:00.1"  # optional if resource pool used
  }'
```

---

### 2. **Custom Init Container or Admission Webhook to Inject a Random NAD**

Since Kubernetes doesn't support "pick a random NAD" natively, your workaround is:

#### Option A: Init Container + Downward API + Host Mount
1. Mount a host path (e.g., `/etc/cni/net.d`) into the pod as read-only.
2. Have an **init container** (or even an entrypoint script) that:
   - Lists available NADs (e.g., via Kubernetes API).
   - Picks one at random.
   - Patches the pod's annotations to include it (if running in privileged mode + with necessary permissions).
   - Or writes the choice to an environment file used by the main container.

#### Option B: Mutating Admission Webhook
This is more elegant but more complex:
- Write a small webhook service (e.g., in Go or Python) that intercepts Pod creation.
- At admission time, it:
  - Queries available NADs matching a pattern (e.g., label or name prefix).
  - Picks one at random.
  - Adds the `k8s.v1.cni.cncf.io/networks` annotation with the chosen NAD.

👉 This works seamlessly and is transparent to your apps.

---

### 3. **Deploy Your Pods With a Generic NAD Label**
You only need to request something like this:

```yaml
metadata:
  annotations:
    k8s.v1.cni.cncf.io/networks: sriov-net-<RANDOM>
```

The actual `<RANDOM>` part gets injected by your webhook or entrypoint logic.

---

### 🛠️ Tools You’ll Need
- Kubernetes client (kubectl or client library)
- Permissions to list NADs (CRD)
- Optional: controller/webhook framework like Kubebuilder or Kopf (Python)
- Custom script or binary to pick the NAD at runtime

---

### ✅ Benefits
- Full control and randomness
- Works even with static SR-IOV assignments
- Extensible for other policies (e.g., "least used")

---

### ⚠️ Caveats
- You'll need a controller/webhook or privileged init logic.
- NADs must be kept in sync with actual node hardware and VF pools.
- If you’re using the `sriov-network-operator`, you may have to manually break up your `SriovNetworkNodePolicy` into per-interface pools.

---

Want a ready-made Python or Bash script to randomly pick a NAD and patch the pod? Or a small webhook template in Go/Python?



Awesome! Here's a **ready-made Bash script** that randomly selects a `NetworkAttachmentDefinition` (NAD) from a specific namespace and outputs it in a way your Pod or init container can use.

---

### ✅ What this script does:
- Lists all NADs in a namespace (with optional prefix filter).
- Randomly picks one.
- Writes it to a file or echoes it (depending on how you want to use it).

---

### 📄 `pick-random-nad.sh`

```bash
#!/bin/bash

# CONFIGURABLE VARIABLES
NAMESPACE="${NAD_NAMESPACE:-default}"        # Namespace where NADs are defined
PREFIX="${NAD_PREFIX:-sriov-net-}"           # Filter NADs with a certain prefix
OUTPUT_FILE="${OUTPUT_FILE:-/tmp/selected_nad}"  # File to store result

# REQUIRE: kubectl installed and available in container
if ! command -v kubectl &> /dev/null; then
    echo "kubectl not found in PATH"
    exit 1
fi

# Fetch list of matching NADs
NADS=$(kubectl get network-attachment-definitions -n "$NAMESPACE" -o jsonpath="{.items[*].metadata.name}" | tr ' ' '\n' | grep "^${PREFIX}")

# Check if we found anything
if [ -z "$NADS" ]; then
    echo "No NADs found in namespace '$NAMESPACE' with prefix '$PREFIX'"
    exit 1
fi

# Pick one at random
SELECTED_NAD=$(echo "$NADS" | shuf -n1)

echo "Selected NAD: $SELECTED_NAD"

# Save to file or stdout
echo "$SELECTED_NAD" > "$OUTPUT_FILE"
```

---

### 🧪 Example Usage in an Init Container (conceptual)

```yaml
initContainers:
  - name: pick-nad
    image: bitnami/kubectl:latest
    command: ["/bin/bash", "-c"]
    args:
      - |
        chmod +x /scripts/pick-random-nad.sh
        /scripts/pick-random-nad.sh
    env:
      - name: NAD_NAMESPACE
        value: "default"
      - name: NAD_PREFIX
        value: "sriov-net-"
    volumeMounts:
      - name: scripts
        mountPath: /scripts
      - name: shared
        mountPath: /nad

containers:
  - name: main-app
    image: your-app
    env:
      - name: NAD_NAME
        valueFrom:
          configMapKeyRef:
            name: nad-config
            key: selected_nad  # Or mount shared volume if using file
    volumeMounts:
      - name: shared
        mountPath: /nad
volumes:
  - name: scripts
    configMap:
      name: pick-nad-script
  - name: shared
    emptyDir: {}
```

---

### 🛡️ Permissions Needed
Your service account (or init container) needs permission to:

```yaml
apiGroups: ["k8s.cni.cncf.io"]
resources: ["network-attachment-definitions"]
verbs: ["list"]
```

---

Awesome — let’s build a **Go-based mutating admission webhook** that adds a random SR-IOV NAD to pod annotations at creation time.

---

## 🧩 Overview

- **Language**: Go
- **Framework**: [`controller-runtime`](https://github.com/kubernetes-sigs/controller-runtime) (standard for webhooks/controllers)
- **Function**: Inject `k8s.v1.cni.cncf.io/networks: sriov-net-<random>` into pods missing that annotation
- **Behavior**: Only mutate pods that match a label (e.g., `inject-sriov: "true"`)
- **Deployment**: Webhook service + `MutatingWebhookConfiguration` + TLS certs

---

## 📦 Project Structure

```
sriov-webhook/
├── main.go
├── webhook.go
├── Dockerfile
├── config/
│   ├── webhook.yaml
│   └── certs/
```

---

## 🧠 Core Go Code

### 📄 `main.go`

```go
package main

import (
	"flag"
	"fmt"
	"log"
	"net/http"
	"os"

	"sriov-webhook/webhook"
)

func main() {
	var certFile, keyFile string
	flag.StringVar(&certFile, "tls-cert-file", "/certs/tls.crt", "Path to TLS certificate file")
	flag.StringVar(&keyFile, "tls-key-file", "/certs/tls.key", "Path to TLS key file")
	flag.Parse()

	mux := http.NewServeMux()
	mux.HandleFunc("/mutate", webhook.HandlePodMutate)

	log.Println("🚀 Starting webhook server on port 8443...")
	server := &http.Server{
		Addr:    ":8443",
		Handler: mux,
	}
	err := server.ListenAndServeTLS(certFile, keyFile)
	if err != nil {
		log.Fatalf("❌ Failed to start server: %v", err)
	}
}
```

---

### 📄 `webhook.go`

```go
package webhook

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"math/rand"
	"net/http"
	"time"

	admissionv1 "k8s.io/api/admission/v1"
	corev1 "k8s.io/api/core/v1"
)

var nadList = []string{
	"sriov-net-ens1f0", "sriov-net-ens2f0", "sriov-net-ens3f0",
}

func HandlePodMutate(w http.ResponseWriter, r *http.Request) {
	body, err := ioutil.ReadAll(r.Body)
	if err != nil {
		http.Error(w, "unable to read request", http.StatusBadRequest)
		return
	}

	review := admissionv1.AdmissionReview{}
	if err := json.Unmarshal(body, &review); err != nil {
		http.Error(w, "unable to parse admission review", http.StatusBadRequest)
		return
	}

	pod := corev1.Pod{}
	if err := json.Unmarshal(review.Request.Object.Raw, &pod); err != nil {
		http.Error(w, "unable to unmarshal pod", http.StatusBadRequest)
		return
	}

	if pod.ObjectMeta.Annotations["k8s.v1.cni.cncf.io/networks"] != "" {
		respond(w, &review, nil) // already has NAD
		return
	}

	if pod.ObjectMeta.Labels["inject-sriov"] != "true" {
		respond(w, &review, nil) // label not matched
		return
	}

	// Pick random NAD
	rand.Seed(time.Now().UnixNano())
	randomNAD := nadList[rand.Intn(len(nadList))]

	patch := []map[string]interface{}{
		{
			"op":    "add",
			"path":  "/metadata/annotations/k8s.v1.cni.cncf.io~1networks",
			"value": randomNAD,
		},
	}

	patchBytes, _ := json.Marshal(patch)

	review.Response = &admissionv1.AdmissionResponse{
		UID:     review.Request.UID,
		Allowed: true,
		Patch:   patchBytes,
		PatchType: func() *admissionv1.PatchType {
			pt := admissionv1.PatchTypeJSONPatch
			return &pt
		}(),
	}

	respBytes, _ := json.Marshal(review)
	w.Header().Set("Content-Type", "application/json")
	w.Write(respBytes)
}

func respond(w http.ResponseWriter, review *admissionv1.AdmissionReview, patch []byte) {
	review.Response = &admissionv1.AdmissionResponse{
		UID:     review.Request.UID,
		Allowed: true,
	}
	respBytes, _ := json.Marshal(review)
	w.Header().Set("Content-Type", "application/json")
	w.Write(respBytes)
}
```

---

## 📦 Dockerfile

```dockerfile
FROM golang:1.21 as builder
WORKDIR /app
COPY . .
RUN go build -o sriov-webhook .

FROM alpine
WORKDIR /root/
COPY --from=builder /app/sriov-webhook .
COPY config/certs /certs
CMD ["./sriov-webhook", "-tls-cert-file=/certs/tls.crt", "-tls-key-file=/certs/tls.key"]
```

---

## 🧾 `MutatingWebhookConfiguration` (snippet)


```
mkdir -p manifests/webhook-sr-iov
cd manifests /web-sr-iov
nano 01-webhook.yaml
```

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: sriov-nad-injector
webhooks:
  - name: nad-injector.sriov.svc.cluster.local 
    rules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
    clientConfig:
      service:
        name: sriov-webhook
        namespace: your-namespace
        path: /mutate
      caBundle: <base64-cabundle>
    admissionReviewVersions: ["v1"]
    sideEffects: None
```

```
kubectl apply -f .
```

---

## 🧪 Deploying

You’ll need:
- TLS certs (can use `cert-manager` or Kubernetes CA)
- A `Deployment` + `Service` for the webhook
- RBAC (if needed to query real NADs from the API)

---

```

Perfect! Here's a helper script that:

1. Generates a **self-signed TLS certificate** for the webhook service.
2. Base64-encodes the **CA bundle**.
3. Prepares it for inclusion in the `MutatingWebhookConfiguration`.

---

## 📄 `generate-tls.sh`

```bash
#!/bin/bash

# Configurable values
NAMESPACE="sriov-webhook"
SERVICE="sriov-webhook"
SECRET_NAME="sriov-webhook-tls"

# Create certs directory
mkdir -p certs && cd certs

# Generate CA
openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes -key ca.key -subj "/CN=SRIOV Webhook CA" -days 365 -out ca.crt

# Generate server key and CSR
openssl genrsa -out tls.key 2048
openssl req -new -key tls.key -subj "/CN=${SERVICE}.${NAMESPACE}.svc" -out server.csr

# Generate server certificate signed by our CA
cat > csr.conf <<EOF
[ v3_ext ]
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = ${SERVICE}
DNS.2 = ${SERVICE}.${NAMESPACE}
DNS.3 = ${SERVICE}.${NAMESPACE}.svc
EOF

openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
    -out tls.crt -days 365 -extensions v3_ext -extfile csr.conf

# Create Kubernetes TLS secret
kubectl create secret tls ${SECRET_NAME} \
  --cert=tls.crt --key=tls.key \
  -n ${NAMESPACE}

# Output base64-encoded CA bundle for webhook
echo
echo "📎 Copy this base64-encoded CA bundle into your MutatingWebhookConfiguration:"
echo
base64 -w 0 ca.crt
echo
```

---

## 🧪 How to Use

1. Save it as `generate-tls.sh`  
2. Run: `chmod +x generate-tls.sh && ./generate-tls.sh`  
3. Copy the printed base64 `caBundle` into your `MutatingWebhookConfiguration.yaml` here:

```yaml
clientConfig:
  caBundle: <PASTE_CA_BUNDLE_HERE>
```

---
```

