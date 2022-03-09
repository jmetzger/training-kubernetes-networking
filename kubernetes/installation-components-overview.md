# Überblick Komponenten Installation 

## Server 1 (manuell installiert -> microk8s)

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

## Server 2+3 (automatische Installation -> microk8s ) 

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
