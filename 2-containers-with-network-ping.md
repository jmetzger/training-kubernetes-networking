# 2 Containers with Network and Ping

```
clear
docker run --name dockerserver1 -dit ubuntu
docker run --name dockerserver2 -dit ubuntu
docker network ls
docker network inspect bridge
# dockerserver1 - 172.17.0.2
# dockerserver2 - 172.17.0.3
docker container ls
docker exec -it dockerserver1 bash
# im container 
apt update; apt install -y iputils-ping 
ping 172.17.0.3 
```
