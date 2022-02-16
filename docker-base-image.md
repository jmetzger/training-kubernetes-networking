# Docker Base Image bauen 

## Auf Basis von debootstrap 

```
# Auf einem Debian oder Ubuntu - System 
# folgende Schritte ausführen 
# z.B. virtualbox -> Ubuntu 20.04. 

## alles mit root durchführen
apt install debootstrap
cd
debootstrap focal focal > /dev/null
tar -C focal -c . | docker import - focal 

# er gibt eine checksumme des images 
# so kann ich das sehen
# müsste focal:latest heissen
docker images

# teilchen starten 
docker run --name my_focal2 -dit focal:latest bash 

# Dann kann ich danach reinwechseln 
docker exec -it my_focal2 bash 
```

## Virtuelle Maschine Windows/OSX mit Vagrant erstellen

```
# Installieren.
https://vagrantup.com 
# ins terminal 
cd 
cd Documents 
mkdir ubuntu_20_04_test 
cd ubuntu_20_04_test
vagrant init ubuntu/focal64
vagrant up 
# Wenn die Maschine oben ist, kann direkt reinwechseln
vagrant ssh 
# in der Maschine kein pass notwendig zum Wechseln 
sudo su -
```


## Ref:

  * https://docs.docker.com/develop/develop-images/baseimages/
