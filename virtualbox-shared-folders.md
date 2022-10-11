# VirtualBox work with shared folders (Ubuntu Server) 

## Prepare 

```
Walkthrough

# At the top menu of the virtual machine 
# Menu -> Geräte -> Gasterweiterung einlegen 

# In the console do a 
mount /dev/cdrom /mnt
cd /mnt
./VBoxGuestAdditions.run 

# Reboot
# Check if it is installed
dpkg -l | grep virtualbox-guest 
usermod -aG vboxsf root 
usermod -aG vboxsf <your-user>
```

## Configure 

```
Geräte -> Gemeinsame Ordner 
Hinzufügen -> 
Ordner-Pfad
```

