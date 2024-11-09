---
title: "Docker"
layout: default
has_children: true
nav_order: 2
---

Some useful commands for Docker

## List and inspect

`docker images`: see all available images\
`docker container ls`: see all docker instances\
`docker ps`: see all docker instances\
`docker stats --all`: see docker usage statistics\
`docker system df -v`: see the docker disk usage\
`docker inspect [id]`: check docker instance specifications\
`docker exec -it <container name> /bin/bash` : connect to a docker instance


## Start a new instance

Start a new docker with port forwardings:
```yaml
docker run --publish 2237:22/tcp --publish 8037:8101/tcp  --detach --name docker_name image_name
```
Here the internal port 22 of the docker is mapped to the port 2237 of the VM where the command is run.\
Same for the port 8181 to port 8137.\
Replace docker_name by the name you want to give to the docker.\
Replace image_name by the name of the image you want to use.

We can limit the number of CPU and memory allocated to a docker, here 2 CPU and 2GB of memory:
```yaml
docker run -h docker_name --cpus "2.0" --memory "2g" -p 2208:22/tcp -p --detach --name docker_name image_name
```
## Create a new image
We can use a running docker to create a new image.\
`docker commit docker_name image_name_2`\
That why, we can instantiate new docker from this image:
```yaml
docker run --publish 2237:22/tcp --detach --name docker_name_2 image_name_2
```

## Stop and Remove a docker instance
`docker stop docker_name`\
`docker rm docker_name`


## Copy a docker image from on server to another
Let's assume that the docker image is local to a machine and we want to export and import the image to another machine:

Save the image in a file: `docker save image_name > image_name.tar`\
Send the file to another server with scp: `scp image_name.tar adrien@192.168.1.10:/home/adrien`\
Load the image on the other server: `docker load < image_name.tar`\
Verify that the image is on the other server: `docker image list`\
We can instantiate the docker (don't forget to add the image version): `docker run --publish 2238:22 --detach --name docker_name image_name:0.4.0`
