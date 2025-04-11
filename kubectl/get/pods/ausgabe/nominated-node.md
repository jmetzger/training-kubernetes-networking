# Was bedeutet die Spalte NOMINATED NODE in der Ausgabe von kubectl get pods 

## Erklärung 

Die Spalte **"NOMINATED NODE"** in der Ausgabe von `kubectl get pods` zeigt an, welcher Node für einen Pod **nominiert** wurde, wenn dieser momentan **nicht planbar** ist, aber **für einen bestimmten Node vorgemerkt wurde**, sobald Ressourcen verfügbar sind.

Das tritt vor allem bei **Preemption**-Fällen auf – also wenn Kubernetes einen Pod mit höherer Priorität bevorzugt und dafür andere (niedriger priorisierte) Pods entfernen muss, um Platz zu schaffen.

### Beispiel-Szenario:
- Du hast einen neuen Pod mit hoher Priorität.
- Der Cluster hat aktuell nicht genug Ressourcen, um ihn sofort zu starten.
- Kubernetes entscheidet, andere Pods auf einem bestimmten Node zu beenden, um Platz zu schaffen.
- Der Node wird dann für den neuen Pod **nominiert** – also vorgemerkt – während die Ressourcen freigeräumt werden.

In diesem Fall wird im Feld **NOMINATED NODE** der Name dieses Nodes angezeigt.

### Kurz gesagt:
- **Leer** = Keine Nominierung → Pod wird normal geplant oder ist bereits auf einem Node.
- **Node-Name steht drin** = Pod wartet darauf, auf genau diesem Node geplant zu werden, sobald Ressourcen verfügbar sind.

## Beispiel 

Perfekt, hier ist ein kleines Beispiel, das zeigt, wie die **`NOMINATED NODE`**-Spalte zustande kommt – und wie man sie beobachten kann. Wir simulieren einen Fall mit **Priority Classes**.

---

### 🛠️ Schritt-für-Schritt-Anleitung

#### 1. Erstelle zwei PriorityClasses

```
cd 
mkdir -p manifests/prio
cd manifests/prio
nano low-priority.yaml
```

```yaml
# low-priority.yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: low-priority
value: 1000
globalDefault: false
description: "Niedrige Priorität"

---
# high-priority.yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 100000
globalDefault: false
description: "Hohe Priorität"
```

```bash
kubectl apply -f low-priority.yaml
kubectl apply -f high-priority.yaml
```

---

#### 2. Erzeuge einen Pod mit niedriger Priorität, der viel CPU braucht

```yaml
# low-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: low-priority-pod
spec:
  priorityClassName: low-priority
  containers:
  - name: busybox
    image: busybox
    args: ["sleep", "3600"]
    resources:
      requests:
        cpu: "1600m"
```

```bash
kubectl apply -f low-pod.yaml
```

> Dieser Pod wird einen großen Teil der CPU auf einem Node belegen.

---

#### 3. Jetzt starte einen **high-priority Pod**, der ebenfalls CPU braucht

```yaml
# high-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: high-priority-pod
spec:
  priorityClassName: high-priority
  containers:
  - name: busybox
    image: busybox
    args: ["sleep", "3600"]
    resources:
      requests:
        cpu: "1200m"
```

```bash
kubectl apply -f high-pod.yaml
```

> Kubernetes merkt: Der Platz reicht nicht, aber der neue Pod hat eine höhere Priorität. Also wird der `low-priority-pod` **preempted** (also entfernt), um Platz zu machen.

---

#### 4. Schau dir die Pods an

```bash
kubectl get pods -o wide
```

Du solltest sehen:

- Der `high-priority-pod` ist noch **Pending**, aber in der Spalte **NOMINATED NODE** steht der Node, den Kubernetes ausgewählt hat, **sobald der Platz frei ist**.
- `low-priority-pod` wird entfernt oder terminiert.

---

Wenn du willst, kann ich dir ein kleines Skript oder ein Minikube-kompatibles Lab vorbereiten, um das direkt auszuprobieren. Interessiert?
