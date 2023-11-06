## General Commands:

* To check version of docker used
```
docker --version
```

* To display system-wide information about Docker
```
docker info
```

* Show help for Docker commands
```
docker --help
```

## Image Commands:

* To pull a image from repo(DockerHub)
```
docker pull <image_name>
```

* To see list of all Docker images present locally
```
docker images
```
```
docker image ls
```

* To build a docket image
```
docker build -t <docker.io>/<username>/<image_name>:tag <path_to_DockerFile>
```

* To remove a docker image locally
```
docker rmi <image_name>
```

## Container Commands: 

* To run a docker image
```
docker run <image_name>
```

* To get list of all running containers
```
docker ps
```

* To get list of all containers including running and stopped
```
docker ps -a
```

* To start a container
```
docker start <container_name/container_id>
```

* To stop a container
```
docker stop <container_name/container_id>
```

* To restart a container
```
docker restart <container_name/container_id>
```

* To execute a command in a running container
```
docker exec -it <container_name/id> <command>
```

* To view logs inside docket container
```
docker logs <container_name/id>
```

* To remove a stopper container
```
docker rm <container_name/id>
```

* To run a docker image with port forwarding
```
docker run -p <host_port>:<container_port> <image_name>
```

* To run a docker image with volume binding
```
docker run -v <host_path/volume>:<container_path> <image_name>
```

* To display detailed information about a container
```
docker inspect <container_name/id>
```

## Networking Commands:

* To create a docker network
```
docker network create <network_name>
```

* To list all avaialbe networks
```
docker network ls
```

## Volume Commands:

* To create a docker volume
```
docker volume create <volume_name>
```

* To list all volume available
```
docker volume ls
```

## Registry and Authentication Commands:

* Log in to a Docker registry
```
docker login
```

* Push an image to a registry
```
docker push <image_name>
```

* Log out from a Docker registry
```
docker logout
```

## Docker Compose Commands:

*  Start services defined in a __docker-compose.yml__ file
```
docker-compose up
```

* Stop and remove containers defined in a __docker-compose.yml__ file
```
docker-compose down
```

* Build or rebuild services defined in a __docker-compose.yml__ file
```
docker-compose build
```

## Debugging Commands:

* ### To check the status of containers
```
docker ps -a
```

* ### Check for container logs
```
docker logs <container_id or container_name>
```

* ### Inspect docker container
```
docker inspect <container_id or container_name>
```

* ### To verify if any issue within container get terminal access to container
```
docker exec -it <container_id or container_name> /bin/bash
```

* ### Monitor Resource Usage
```
docker stats <container_id or container_name>
```

* ### Check Container Network Configuration

* ### Check Environment Variables

* ### Check Volume Mounts




