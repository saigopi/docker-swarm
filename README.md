# docker-swarm
Intro to DockerSwarm Setup the cluster using the Vagrant and initialised the swarm 
```
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env master01
 gopinathjayabalan@IND-CHN-gopinath  ~/code  docker-machine env master01
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/gopinathjayabalan/.docker/machine/machines/master01"
export DOCKER_MACHINE_NAME="master01"
# Run this command to configure your shell:
# eval $(docker-machine env master01)
 gopinathjayabalan@IND-CHN-gopinath  ~/code  eval $(docker-machine env master01)
 gopinathjayabalan@IND-CHN-gopinath  ~/code 
```
###### Connect to swarm master - Docker-Machine environment
```
docker-machine ssh master01
```
Initialize Docker Swarm master
```
docker swarm init --advertise-addr 192.168.99.100
```
#####Dockerfile 
```
#simple web server
FROM centos:latest

LABEL maintainer="Sri/Gopi"

RUN yum install -y httpd

RUN echo "Our Container Website >> /var/www/html/index.html"

EXPOSE 80

ENTRYPOINT apachectl -DFOREGROUND
```



#####Docker-compose file 
######note: Run this compose file where the Dockerfile need to be present.  
```
version:3
service: 
  apiweb01:
    image: my-image-web-01
    build: .
    ports: 
      - "81:80"
  apiweb02:
    image: my-image-web-01
    port:
      - "82:80"
  load-blancer:
    image: nginx:latest
    ports: 
      - "80:80"
```




```
docker service update --publish-add 8080:80 <service-name>
```
###### update the service and see the output progress in console
```
docker service update --detach=false replicas=3 
```
###### CPU and Memory limit - soft and hard limit
##### NOTE: Applying the CPU and Memory limit restart.
```
docker service update --limit-cpu=.5 --reserve-cpu=.5 --limit-memory=64m --reserve-memory=128m --detach=true service-name
```
###### There are a couple of ways to handle scaling in a running service.
##### Replicas:
######Docker scale up only on replicated mode
***Global and*** 
***Replicated***
```
docker service create --name myserice -p 8080:80 --mode global --detach=false nginx
```
```
docker service create --name testweb -p 8080:80 httpd  
```
###### update the replicas for service in single command. "In docker update replica=3 service1 service2" its not possible
###### Hence use docker scale command
```
docker service scale --detach=false service-1=3 service-2=5
```
Node Constraints
```
docker service creat --name name-given-node-constraints -p 8080:80 --constraint 'node.labels.mynode == testnode' --replica 5 httpd
```

****Troubleshoot:****
Active Internet connections
Active UNIX domain sockets
```
netstat tulp
```

*****READ THIS FOR REFERENCE*****
```
https://medium.com/devopsion/solving-the-routing-mess-for-services-in-docker-73492c37b335

https://upcloud.com/community/tutorials/docker-swarm-orchestration/
```
****Docker Documentation****
```
https://docs.docker.com/v17.09/develop/
SWARM Service
https://docs.docker.com/v17.09/get-started/part4/#restarting-docker-machines
Simple Python App TO get the container ID
https://docs.docker.com/v17.09/get-started/part2/#run-the-app
```

***ENSURE***
If we you created in Replica mode.. Can you change to Global ? and Viseversa 
