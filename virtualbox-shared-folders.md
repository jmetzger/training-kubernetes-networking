# VirtualBox work with shared folders (Ubuntu Server) 

## Prepare 

```
Walkthrough

# At the top menu of the virtual machine 
# Menu -> Geräte -> Gasterweiterung einlegen 

# In the console do a 
mount /dev/cdrom /mnt
cd /mnt
sudo ./VBoxGuestAdditions.run 

# Reboot
# Check if it is installed
dpkg -l | grep virtualbox-guest 
sudo usermod -aG vboxsf root 
sudo usermod -aG vboxsf <your-user>

sudo reboot

```

## Configure 

```
Geräte -> Gemeinsame Ordner 
Hinzufügen (blaues Ordnersymbol mit + ) -> 
Ordner-Pfad: C:\Linux (Ordner muss auf Windows angelegt sein) 
Ordner-Name: linux
checkbox nicht ausgewählt bei : automatisch einbinden, nur lesbar
checkbox ausgewählt bei: Permanent erzeugen

Dann rebooten

In der virtuellen Maschine:  
sudo su -
mkdir /linux
# linux ist der vergebene Ordnername 
mount -t vboxsf linux /linux 
```


## persistent setzen (beim booten mounten) 
```
echo "linux	/linux	vboxsf	defaults	0	0" >> /etc/fstab 
reboot
```

## Reference:

  * https://gist.github.com/estorgio/1d679f962e8209f8a9232f7593683265
