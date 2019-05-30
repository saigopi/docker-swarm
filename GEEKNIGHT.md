####### To build image using dockerfile #s
```
docker build -t hello-world -f dockerfile .

docker run -p 8080:80 hello-world

localhost:8080

docker ps

docker exec -it <container-name> sh

ls
```
###### Start the Docker-compose #S
```
docker compose up

localhost:81

docker compose down

docker ps
```

######To set up the docker swarm #S
```
docker-machine create --driver virtualbox master01
docker-machine create --driver virtualbox worker01
docker-machine create --driver virtualbox worker02
docker-machine create --driver virtualbox worker03

```
###### To setup the environment variables #S

```
docker-machine ssh master01

docker-machine ssh worker01

docker-machine ssh worker02

docker-machine ssh worker03

```

####### Initialize Docker Swarm master #G
```
docker swarm init --advertise-addr 192.168.99.100
```

###### Get the swarm token from the master #G
```
docker swarm join-token worker
```

###### Put the token generated in the worker

###### Create a network in the master #S
```
docker network create -d overlay  development

docker network ls
```

###### Create a secret in the master #S
```
printf "test" | docker secret create dev_env_appdb01 -

docker secret ls
```

###### Create a config in the master #S
```
docker config create dev-config config.js 

docker config ls
```

###### Start the Docker-stack using the same compose file #G

```
docker stack deploy -c 01-db-demo.yml development

docker stack deploy -c 02-ui-demo.yml development

docker stack deploy -c 01-api-demo.yml development

```
###### #S
```
docker stack ls

docker service ls

docker service ps <service-id>

docker ps

docker exec -it <api-container-id> bash

```
###### Ping from api container 
```
ping api
```

###### To show the output in browser 
```
192.168.99.100:8080
```
