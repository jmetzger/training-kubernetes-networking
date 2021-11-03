# Docker swarm examples 

## Generic examples 

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

```
# Start with multiple replicas and name 
docker service create --name my_redis --replicas 4 redis
docker service ls
# Welche tasks 
docker service ps my_redis
docker container ls
docker service inspect my_redis

# delete service
docker service rm
```

## Add additional node 

```
# on first node, get join token 
docker swarm join-token manager

# on second node execute join command
docker swarm join --token SWMTKN-1-07jy3ym29au7u3isf1hfhgd7wpfggc1nia2kwtqfnfc8hxfczw-2kuhwlnr9i0nkje8lz437d2d5 192.168.56.101:2377

# check with node command
docker node ls 

# Make node a simple worker
# Does not make, because no highavailable after crush node 1
# Take at LEAST 3 NODES 
docker node demote <node-name>

```

## Ref 

  * https://docs.docker.com/engine/swarm/services/
