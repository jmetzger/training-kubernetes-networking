# Was zeigt Readiness Gate an ? 

## Erklärung

Der Eintrag **`Readiness Gates`** in der Ausgabe von `kubectl get pods` bezieht sich auf zusätzliche Bedingungen, die Kubernetes beim **Readiness-Status eines Pods** berücksichtigt – zusätzlich zu den üblichen Container-Readiness-Probes.

### Hintergrund
Normalerweise ist ein Pod **bereit (Ready)**, wenn:
- Alle Container im Pod den Zustand `Ready` erreicht haben.

Mit **Readiness Gates** kannst du jedoch sagen: *"Ein Pod ist erst dann bereit, wenn zusätzlich bestimmte Bedingungen erfüllt sind, die nicht direkt von den Containern selbst kommen."*

Diese Bedingungen werden durch Kubernetes **Conditions** im `Pod.status.conditions`-Feld angegeben – zum Beispiel von einem **Controller**, der auf externe Systeme wartet (z. B. Netzwerk-Bereitstellung, Secrets Injection, etc.).

---

### Beispiel:

```yaml
readinessGates:
  - conditionType: "example.com/ready"
```

Dann muss im `status.conditions` des Pods ein Eintrag wie folgt auftauchen:

```yaml
status:
  conditions:
    - type: "example.com/ready"
      status: "True"
```

Solange dieser Zustand **nicht auf `True`** gesetzt wurde, bleibt der Pod **nicht "Ready"**, auch wenn alle Container bereit sind.

---

### kubectl-Ausgabe
Wenn du `kubectl get pods` verwendest, kann es eine Spalte `READINESS GATES` geben (z. B. mit `kubectl get pods -o wide` oder durch eigene Spalten mit `-o custom-columns`), aber standardmäßig sieht man das nicht immer direkt. Was du aber sehen kannst:

```bash
kubectl get pod <pod-name> -o jsonpath='{.status.conditions[*].type}'
```

Oder detaillierter mit:
```bash
kubectl describe pod <pod-name>
```
Dort siehst du die Readiness Gates und ob sie erfüllt sind.

---

### Fazit
**Readiness Gates** erlauben feinere Kontrolle darüber, wann ein Pod als "Ready" betrachtet wird – sie sind **eine erweiterbare Schnittstelle für zusätzliche externe Bedingungen**. Besonders nützlich bei:

- Service-Meshes (z. B. Istio, Linkerd)
- Netzwerkbereitstellung (z. B. Multus)
- Secrets-/Config-Injection
- Custom Controller-Logik

---

## Beispiel 2 


---

### 🧪 Ziel:  
Der Pod soll erst dann als „Ready“ gelten, wenn eine benutzerdefinierte Condition (`example.com/ready`) auf `True` gesetzt wurde.

---

### 🔧 Deployment-Beispiel mit Readiness Gate

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: readiness-gate-demo
spec:
  readinessGates:
    - conditionType: "example.com/ready"
  containers:
    - name: app
      image: busybox
      args: ["sh", "-c", "echo App running; sleep 3600"]
      readinessProbe:
        exec:
          command: ["cat", "/tmp/ready"]
        initialDelaySeconds: 3
        periodSeconds: 5
  initContainers:
    - name: set-readiness
      image: busybox
      command:
        - sh
        - -c
        - |
          echo "Waiting 10s before setting readiness condition..."
          sleep 10
          kubectl patch pod readiness-gate-demo \
            --type='json' \
            -p='[{"op":"add","path":"/status/conditions/-","value":{"type":"example.com/ready","status":"True","lastTransitionTime":"'$(date -Iseconds)'"}}]' \
            --subresource=status
```

---

### 📝 Erklärung:

- `readinessGates`: definiert, dass der Pod nur „Ready“ wird, wenn die Condition `example.com/ready` erfüllt ist.
- `initContainer`: wartet 10 Sekunden und setzt dann die Readiness-Condition per `kubectl patch`.
- Die Hauptcontainer-Readiness-Probe (`readinessProbe`) erwartet eine Datei `/tmp/ready`, die wir hier nur symbolisch drin haben.

---

### ❗ Voraussetzung:
Damit der `initContainer` `kubectl` verwenden kann und auf den API-Server zugreifen darf, müsstest du:
- Ein ServiceAccount mit passenden RBAC-Rechten zuweisen
- Oder du simulierst das manuell von außen (z. B. durch ein Sidecar oder externen Controller)

---

### 🔍 Beobachtung:

```bash
kubectl get pod readiness-gate-demo -w
```

Du wirst sehen, dass der Pod **nicht "READY"** ist, bis die Bedingung erfüllt ist.

---

Möchtest du auch ein RBAC-Beispiel dazu, damit das `kubectl patch` im Container funktioniert?
Wenn du magst, kann ich dir ein Minibeispiel zeigen, wie man so einen Readiness Gate in einem Deployment nutzt.
