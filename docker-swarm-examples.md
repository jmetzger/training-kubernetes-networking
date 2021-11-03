# Docker swarm examples 

```
# should be at least version 1.24 
docker info

# only for one network interface
docker swarm init

# in our case, we need to decide what interface
docker swarm init --advertise-addr 192.168.56.101

# is swarm active 
docker info | grep -i swarm
# When it is -> node command works 
docker node ls
# is the current node the manager 
docker info | grep -i "is manager"

# docker create additional overlay network 
docker network ls

# what about my own node -> self
docker node inspect self
docker node inspect --pretty self
docker node inspect --pretty self | less

```

```
# Create our first service 
docker service create redis
docker images
docker service ls
# if service-id start with  j 
docker service inspect j
docker service ps j
docker service rm j
docker service ls
```
