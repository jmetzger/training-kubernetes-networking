# Überblick Komponenten Installation 

## Step 1: Server 1 (manuell installiert -> microk8s)

```
# Installation Ubuntu - Server 

# cloud-init script 
# s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

# Server 1 - manuell 
# Ubuntu 20.04 LTS - Grundinstallation 

# minimal Netzwerk - öffentlichen IP 
# nichts besonderes eingerichtet - Standard Digitalocean 

# Standard vo Installation microk8s 
lo               UNKNOWN        127.0.0.1/8 ::1/128
# public ip / interne 
eth0             UP             164.92.255.234/20 10.19.0.6/16 fe80::c:66ff:fec4:cbce/64
# private ip 
eth1             UP             10.135.0.3/16 fe80::8081:aaff:feaa:780/64

snap install microk8s --classic 
# namensaufloesung fuer pods 
microk8s enable dns 

```

```
# Funktioniert microk8s 
microk8s status
```

## Steps 2: Server 2+3 (automatische Installation -> microk8s ) 

```
# Was macht das ? 
# 1. Basisnutzer (11trainingdo) - keine Voraussetzung für microk8s
# 2. Installation von microk8s 
#.>>>>>>> microk8s installiert <<<<<<<<
# - snap install --classic microk8s 
# >>>>>>> Zuordnung zur Gruppe microk8s - notwendig für bestimmte plugins (z.B. helm)  
# usermod -a -G microk8s root 
# >>>>>>> Setzen des .kube - Verzeichnisses auf den Nutzer microk8s -> nicht zwingend erforderlich 
# chown -r -R microk8s ~/.kube 
# >>>>>>> REQUIRED .. DNS aktivieren, wichtig für Namensauflösungen innerhalb der PODS
# >>>>>>> sonst funktioniert das nicht !!! 
# microk8s enable dns 
# >>>>>>> kubectl alias gesetzt, damit man nicht immer microk8s kubectl eingeben muss
# - echo "alias kubectl='microk8s kubectl'" >> /root/.bashrc

# cloud-init script 
# s.u. MITMICROK8S (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 
#cloud-config
users:
  - name: 11trainingdo
    shell: /bin/bash

runcmd:
  - sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
  - echo " " >> /etc/ssh/sshd_config 
  - echo "AllowUsers 11trainingdo" >> /etc/ssh/sshd_config 
  - echo "AllowUsers root" >> /etc/ssh/sshd_config 
  - systemctl reload sshd 
  - sed -i '/11trainingdo/c 11trainingdo:$6$HeLUJW3a$4xSfDFQjKWfAoGkZF3LFAxM4hgl3d6ATbr2kEu9zMOFwLxkYMO.AJF526mZONwdmsm9sg0tCBKl.SYbhS52u70:17476:0:99999:7:::' /etc/shadow
  - echo "11trainingdo ALL=(ALL) ALL" > /etc/sudoers.d/11trainingdo
  - chmod 0440 /etc/sudoers.d/11trainingdo
  
  - echo "Installing microk8s"
  - snap install --classic microk8s
  - usermod -a -G microk8s root
  - chown -f -R microk8s ~/.kube
  - microk8s enable dns 
  - echo "alias kubectl='microk8s kubectl'" >> /root/.bashrc
```
```
# Prüfen ob microk8s - wird automatisch nach Installation gestartet
# kann eine Weile dauern
microk8s status

```

## Step 3: Client - Maschine (wir sollten nicht auf control-plane oder cluster - node arbeiten

```
Weiteren Server hochgezogen. 
Vanilla + BASIS 

# Installation Ubuntu - Server 

# cloud-init script 
# s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

# Server 1 - manuell 
# Ubuntu 20.04 LTS - Grundinstallation 

# minimal Netzwerk - öffentlichen IP 
# nichts besonderes eingerichtet - Standard Digitalocean 

# Standard vo Installation microk8s 
lo               UNKNOWN        127.0.0.1/8 ::1/128
# public ip / interne 
eth0             UP             164.92.255.232/20 10.19.0.6/16 fe80::c:66ff:fec4:cbce/64
# private ip 
eth1             UP             10.135.0.5/16 fe80::8081:aaff:feaa:780/64

```

```
#### Installation von kubectl aus dem snap
# NICHT .. keine microk8s - keine control-plane / worker-node 
# NUR Client zum Arbeiten 
snap install kubectl --classic 

#### .kube/config 
# Damit ein Zugriff auf die kube-server-api möglich
# d.h. REST-API Interface, um das Cluster verwalten.
# Hier haben uns für den ersten Control-Node entschieden
# Alternativ wäre round-robin per dns möglich 

# Mini-Schritt 1:
# Auf dem Server 1: kubeconfig ausspielen 
microk8s config > /root/kube-config 
# auf das Zielsystem gebracht (client 1) 
scp /root/kubeconfig 11trainingdo@10.135.0.5:/home/11trainingdo

# Mini-Schritt 2:
# Auf dem Client 1 (diese Maschine) kubeconfig an die richtige Stelle bringen 
# Standardmäßig der Client nach eine Konfigurationsdatei sucht in ~/.kube/config 
sudo su -
cd 
mkdir .kube 
cd .kube 
mv /home/11trainingdo/kube-config config 

# Verbindungstest gemacht
# Damit feststellen ob das funktioniert. 
kubectl cluster-info 

```

## Schritt 4: Auf allen Servern IP's hinterlegen und richtigen Hostnamen überprüfen 

```
# Auf jedem Server 
hostnamectl 
# evtl. hostname setzen 
# z.B. - auf jedem Server eindeutig 
hostnamectl set-hostname n1.training.local 

# Gleiche hosts auf allen server einrichten.
# Wichtig, um Traffic zu minimieren verwenden, die interne (private) IP

/etc/hosts 
10.135.0.3 n1.training.local n1
10.135.0.4 n2.training.local n2
10.135.0.5 n3.training.local n3 

```

## Schritt 5: Cluster aufbauen 

```
# Mini-Schritt 1:
# Server 1: connection - string (token) 
microk8s add-node 
# Zeigt Liste und wir nehmen den Eintrag mit der lokalen / öffentlichen ip
# Dieser Token kann nur 1x verwendet werden und wir auf dem ANDEREN node ausgeführt
# microk8s join 10.135.0.3:25000/e9cdaa11b5d6d24461c8643cdf107837/bcad1949221a

# Mini-Schritt 2:
# Dauert eine Weile, bis das durch ist. 
# Server 2: Den Node hinzufügen durch den JOIN - Befehl 
microk8s join 10.135.0.3:25000/e9cdaa11b5d6d24461c8643cdf107837/bcad1949221a

# Mini-Schritt 3:
# Server 1: token besorgen für node 3
microk8s add-node 

# Mini-Schritt 4:
# Server 3: Den Node hinzufügen durch den JOIN-Befehl 
microk8s join 10.135.0.3:25000/09c96e57ec12af45b2752fb45450530c/bcad1949221a

# Mini-Schritt 5: Überprüfen ob HA-Cluster läuft 
Server 1: (es kann auf jedem der 3 Server überprüft werden, auf einem reicht 
microk8s status | grep high-availability 
high-availability: yes 
```

## Ergänzend nicht notwendige Scripte 

```
# cloud-init script 
# s.u. BASIS (keine Voraussetzung - nur zum Einrichten des Nutzers 11trainingdo per ssh) 

# Digitalocean - unter user_data reingepastet beim Einrichten 

#cloud-config
users:
  - name: 11trainingdo
    shell: /bin/bash

runcmd:
  - sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
  - echo " " >> /etc/ssh/sshd_config 
  - echo "AllowUsers 11trainingdo" >> /etc/ssh/sshd_config 
  - echo "AllowUsers root" >> /etc/ssh/sshd_config 
  - systemctl reload sshd 
  - sed -i '/11trainingdo/c 11trainingdo:$6$HeLUJW3a$4xSfDFQjKWfAoGkZF3LFAxM4hgl3d6ATbr2kEu9zMOFwLxkYMO.AJF526mZONwdmsm9sg0tCBKl.SYbhS52u70:17476:0:99999:7:::' /etc/shadow
  - echo "11trainingdo ALL=(ALL) ALL" > /etc/sudoers.d/11trainingdo
  - chmod 0440 /etc/sudoers.d/11trainingdo
```
