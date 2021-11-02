# Ubuntu mit ping 

```
mkdir myubuntu 
cd myubuntu/


# nano Dockerfile
FROM ubuntu:latest
RUN apt-get update; apt-get install -y inetutils-ping
CMD ["/bin/bash"]



docker build -t myubuntu .
docker images
docker run -d -t --name container-ubuntu myubuntu
docker container ls
# in den container reingehen mit dem namen des Containers: myubuntu 
docker exec -it myubuntu bash
ls -la
 
```
