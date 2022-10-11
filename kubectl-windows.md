# Kubectl unter windows installieren (microk8s) 

## Walkthrough (Installation)

```
# Step 1
chocolatry installiert. 
(powershell als Administrator ausführen)
# https://docs.chocolatey.org/en-us/choco/setup
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Step 2
choco install kubernetes-cli 

# Step 3
testen:
kubectl version --client 

# Step 4:
# powershell als normaler benutzer öffnen 
```

## Walkthrough (autocompletion) 

```
in powershell (normaler Benutzer) 
kubectl completion powershell | Out-String | Invoke-Expression
```

## kubectl - config - Struktur vorbereiten  

```
# in powershell im heimatordner des Benutzers .kube - ordnern anlegen
# C:\Users\<dein-name>\
mkdir .kube 
cd .kube 
```

## IP von Cluster-Node bekommen 

```
# auf virtualbox - maschine per ssh einloggen 
# öffentliche ip herausfinden - z.B. enp0s8 bei HostOnly - Adapter
ip -br a 
```

## config für kubectl aus Cluster-Node auslesen (microk8s) 

```
# auf virtualbox - maschine per ssh einloggen / zum root wechseln 
# abfragen
microk8s config 

# Alle Zeilen ins clipboard kopieren
# und mit notepad++ in die Datei \Users\<dein-name>\.kube\config 
# schreiben

# Wichtig: Zeile cluster -> clusters / server 
# Hier ip von letztem Schritt eintragen:
# z.B. 
Server: https://192.168.56.106/......
```

## Testen 

```
# in powershell
# kann ich eine Verbindung zum Cluster aufbauen ? 
kubectl cluster-info 
```



  * https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
