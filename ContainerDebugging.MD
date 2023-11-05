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