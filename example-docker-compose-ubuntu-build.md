# Example Docker Compose (Ubuntu with Dockerfile) 

```
cd
mkdir bautest
cd bautest 
# nano docker-compose.yml
version: "3.8"

services:
  myubuntu:
    build: ./myubuntu
    restart: always

```
