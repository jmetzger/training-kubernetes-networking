# Ubuntu ssh-server 

```
cd 
mkdir devubuntu
cd devubuntu 
# vi Dockerfile 
FROM ubuntu:latest
RUN apt-get update; apt-get install -y inetutils-ping; DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata; apt-get install -y openssh-server
EXPOSE 22/tcp
CMD ["/usr/sbin/sshd","-D"]



docker build -t devubuntu . 
docker run --name=devjoy -p 2222:22  -d -t devubuntu3


```
