# Docker Notes
- Docker is open source software to pack, ship and run any application as a lightweight container
- Containers are completely hardware and platform independent so you don’t have to worry about whether what you are creating will run everywhere.
- virtual machines have been used to accomplish many if these same goals. However, Docker containers are smaller and have far less overhead than VMs
- VMs were not built with software developers in mind; they contain no concept of versioning, and logging/monitoring is very difficult. Docker images, on the other hand, are built from layers that can be version controlled. Docker has logging functionality readily available for use.
-  what could go into a “container”? (Well anything)
    - isolate pieces of your system into separate containers
    - potentially have a container for nginx, a container for MongoDB, and one for Redis
- Containers are very easy to setup. Major projects like nginx, MongoDB, and Redis all offer free Docker images for you to use
    - you can install and run any of these containers with just one shell command.
- When distributing Docker images, you should carefully optimize your layers to keep them as small as possible. This tutorial does not cover layer optimization.
- You might be thinking that this is somewhat messy since your container is basically a black box. What if you want to redo your image? Would you have to write down the steps to reproduce the entire thing? What if you wanted to recreate your image from CentOS instead of Ubuntu? Your thinking would be correct. Creating Docker images in this way is not the best idea. Instead you should use Dockerfiles.

## Useful Commands
`docker ps -a` # gives a list of of our docker containers, running or stopped

`docker images` # view a list of images

`docker rmi <image-name>` #removes image

### Removing Docker Images
```
[conicoll@bur3-l1042433m]:mydockerbuild$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
cdnicoll/my-redis   latest              912568c879f3        4 days ago          434.2 MB
redis               latest              34ab544cb5eb        4 days ago          434.5 MB
<none>              <none>              9d1bf76cea70        4 days ago          434.2 MB
ubuntu              latest              2fa927b5cdd3        3 weeks ago         122 MB
docker/whalesay     latest              6b362a9f73eb        13 months ago       247 MB
[conicoll@bur3-l1042433m]:mydockerbuild$ docker rmi -f 6b362a9f73eb
Untagged: docker/whalesay:latest
Deleted: sha256:6b362a9f73eb8c33b48c95f4fcce1b6637fc25646728cf7fb0679b2da273c3f4
```

### To run commands within an image
>`docker exec` allows to execute a command in a running container, `-t` attaches a terminal and `-i` makes it interactive. Finally, `/bin/bash` is the command that is run and creates a bash instance inside the container

```
[conicoll@bur3-l1042433m]:php$ docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                      PORTS                         NAMES
f6ecf103bf16        dockertutorial_nginx    "nginx -g 'daemon off"   15 hours ago        Up 15 hours                 0.0.0.0:80->80/tcp, 443/tcp   dockertutorial_nginx_1
0b99e878c0c8        dockertutorial_php      "php-fpm"                15 hours ago        Up 15 hours                 9000/tcp                      dockertutorial_php_1
5ed4621889a1        mysql:latest            "docker-entrypoint.sh"   15 hours ago        Up 15 hours                 3306/tcp                      dockertutorial_mysql_1
b29b8f576d80        mysql:latest            "docker-entrypoint.sh"   15 hours ago        Exited (0) 15 hours ago                                   dockertutorial_data_1
961ebeeaf38a        php:7.0-fpm             "true"                   15 hours ago        Exited (0) 15 hours ago                                   dockertutorial_app_1
[conicoll@bur3-l1042433m]:php$ docker exec -it 5ed4621889a1 /bin/bash
```

### Starting compose
`docker-compose up -d`

### To view running proccess
`docker ps -a`

### killing docker image
```
docker ps -a
docker stop 3483b534612c
```

### Running commands within a docker-compose directory
> These need to be run within a project that uses docker-compose.yml
#### View open processes
`docker-compose ps`
#### Stopping all containers
`docker-compose stop`
#### Start all containers
`docker-compose up -d`
#### Remove all containers
`docker-compose rm`

### View machine ip
`docker-machine ip`

## Troubleshooting
> display the logs for the containers of the current docker-compose.yml file. You can also run docker-compose ps and check the State column: if there is an exit code different than 0, there was a problem with the container.


`docker-compose logs`

> Display the logs specific to a container

`docker logs <container_id>`

## Errors
### Bug with iTerm
iTerm3 wont support quick luanch, to get around it, run quick launch within terminal, then within iTerm enter `eval $(docker-machine env default)`

## Further Questions
- How does running `docker run --name my-redis -it ubuntu:latest bash` differ from creating a Dockerfile and running `docker run -d -p 6379:6379 redis`

## Resources
https://scotch.io/tutorials/getting-started-with-docker

Pushing docker to repo:
https://docs.docker.com/mac/step_six/
