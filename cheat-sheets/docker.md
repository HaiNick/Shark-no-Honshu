# run containers with docker because vms are overkill. example: spawn shells, copy files, expose ports

## container basics
```bash
docker ps                          # running containers
docker ps -a                       # all containers 
docker images                      # local images
docker run -it ubuntu bash         # interactive shell
docker run -d nginx                # background daemon
docker run --rm alpine ls          # run and delete after
docker run --name web nginx        # named container
```

## container lifecycle
```bash
docker stop <id>                   # stop container
docker start <id>                  # start stopped container
docker restart <id>                # restart container
docker rm <id>                     # delete container
docker rm -f <id>                  # force delete running container
docker kill <id>                   # kill container process
```

## exec into containers
```bash
docker exec -it <id> bash          # bash shell (if exists)
docker exec -it <id> sh            # sh shell (lighter)
docker exec -it <id> /bin/ash      # alpine shell
docker exec -u root -it <id> bash  # exec as root user
docker attach <id>                 # attach to main process
```

## file operations  
```bash
docker cp <id>:/path/file .        # copy file from container
docker cp ./file <id>:/path/       # copy file to container
docker cp <id>:/src ./dst          # copy directory from container
docker cp ./src <id>:/dst/         # copy directory to container
```

## port mapping and networking
```bash
docker run -p 80:80 nginx          # map port 80
docker run -p 127.0.0.1:80:80 nginx # bind to localhost only
docker run --network host nginx    # use host network
docker network ls                  # list networks
docker network create mynet        # create custom network
docker run --network mynet nginx   # use custom network
```

## image management
```bash
docker pull ubuntu:20.04          # pull specific tag
docker build -t myapp .           # build from dockerfile
docker build -t myapp:v1.0 .      # build with tag
docker push myuser/myapp          # push to registry
docker rmi <image>                # delete image
docker tag <image> <newtag>       # tag image
```

## volumes and mounts
```bash
docker run -v /host:/container nginx    # bind mount
docker run -v myvolume:/data alpine     # named volume
docker run --mount type=bind,src=/host,dst=/container nginx
docker volume ls                        # list volumes
docker volume create myvolume           # create volume
docker volume rm myvolume              # delete volume
```

## debugging and inspection
```bash
docker logs <id>                   # view container logs
docker logs -f <id>                # follow logs
docker inspect <id>                # full container details
docker top <id>                    # processes in container
docker stats                       # live resource usage
docker stats <id>                  # specific container stats
docker port <id>                   # port mappings
```

## dockerfile examples
```dockerfile
FROM alpine:latest
RUN apk add --no-cache curl
WORKDIR /app
COPY . .
EXPOSE 8080
CMD ["./app"]
```

## cleanup operations
```bash
docker system prune                # remove unused data
docker system prune -a             # [!] remove all unused images
docker container prune             # remove stopped containers
docker image prune                 # remove dangling images
docker volume prune                # remove unused volumes
docker system df                   # show disk usage
```

## quick one-liners
```bash
docker run --rm -v $(pwd):/data alpine ls /data    # list current dir
docker run --rm -it python:3 python                # python repl
docker run --rm -v ~/.ssh:/root/.ssh alpine        # mount ssh keys
docker exec -it $(docker ps -q) bash               # exec into first running container
```

## danger zone [!]
```bash
docker rm -f $(docker ps -aq)      # [!] delete all containers
docker rmi -f $(docker images -q)  # [!] delete all images
docker system prune -af --volumes  # [!] nuke everything docker
```

# TODO: add docker-compose examples, multi-stage builds, security best practices
