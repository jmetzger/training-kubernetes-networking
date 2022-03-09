# Mehrere Instanzen (replicas in docker-compose) 

## Beispiel 

```
version: "3.9"
services:
  redis:
    image: redis:latest
    deploy:
      replicas: 1
    configs:
      - my_config
      - my_other_config
configs:
  my_config:
    file: ./my_config.txt
  my_other_config:
    external: true
```
## Ref:

  * https://docs.docker.com/compose/compose-file/compose-file-v3/
