# Security Overview 

## Run container under specific user: 

```
# user with id 40000 does not need to exist in container 
docker run -it -u 40000 alpine 

# user kurs needs to exist in container (/etc/passwd) 
docker run -it -u kurs alpine 

```

## Default capabilities 

  * Set everytime a new container is started as default 
  * https://github.com/moby/moby/blob/master/profiles/seccomp/default.json


## Run container with less capabilities 

```
cd
mkdir captest
cd captest 
```

```
nano docker-compose.yml 
```

```
services: 
  nginx:
    image: nginx 
    cap_drop:
      - CHOWN
```

```
docker compose up -d
# start and exits 
docker compose ps 
# 
docker exec -it captest_nginx_1 bash 
#/ touch /tmp/foo; chown 10000 /tmp/foo  

# what happened -> wants to do chown, but it is not allowed 
docker logs captest_nginx_1 

```

```
docker compose down 
```


## Reference:

  * https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
  * https://www.redhat.com/en/blog/secure-your-containers-one-weird-trick
  * man capabilities
