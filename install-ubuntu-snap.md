# Install docker (ubuntu) with snap

```
snap install docker

# for information retrieval 
snap info docker
systemctl list-units
systemctl list-units -t service
systemctl list-units -t service | grep docker

systemctl status snap.docker.dockerd.service


```
