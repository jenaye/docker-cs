# Docker cheat sheet

List your containers

```
docker container ls  # running

docker container ls -a  # running and stopped

docker container ls -q # show running container of the id
```

# Inspect container

```
docker container inspect ID_CONTAINER # Watch configuration of the container and of the host and configuration of the network

FILTER INSPECT :

docker container inspect --format '{{ .Id }}'  ID_CONTAINER # get container id

docker container inspect --format '{{ .NetworkSetting.IPAddress}}' ID_CONTAINER  # get ip address from container

docker container inspect --format '{{ json .State }}' ID_CONTAINER | jq    # get state of the container (online/down)


```

# Log containers

```
docker container logs -F ID_CONTAINER   # get log from container; flag "-F" mean tail -f (real time)
```


# Common command

```
docker container run -d -p 8080:80 nginx  # running container with nginx (port 80 inside) and 8080 outside (you can add "-P" to get alea port)


docker container exec -ti ID_CONTAINER sh # get shell from container

docker container stop ID_CONTAINER # simply stop container

docker container stop $(docker container ls -q) # a trick for shutting down everything

docker port # show all of the ports reachable from outside

```


#For deleting a container :

```

docker container rm ID_CONTAINER  # to delete container. WARNING : the state of container must be "shutdown" to use this command (or, instead, you should use flag "-f")

docker container rm $(docker container ls -aq) # tricks for deleting all of the containers

```


# IMAGES

```
docker image ls # show all of the available images

docker image pull alpine # get the " alpine "

docker image pull mhart/alpine-node  # by default, pull command take the lasted image. Here we download mhart alpine + nodejs

docker image ls node # We can also sort images by name

docker image ls --filter dangling=true #  

docker image prune  # delete all of the useless images

```

# Save / Load

```

# here, an exemple to save image to .tar with tag (name+version)
docker save -o kahz.tar NAME_IMAGE:VERSION_IMAGE

# simply load the image by archive
docker load kahz.tar

```


# Build image from dockerfile

```
docker image build -t Kahz:v1.0  . # here, "." = path of the docker's file. The version isn't required

docker container run -p 8080:80 Kahz # When your build finished, you can run it with this command

```


# Volume
```

1 . /usr/share/nginx/html  # Define the volume in dockerfile
2 . docker container run -v /usr/share/nginx/html IMAGE_NAME # we can also define it when we are running the container

( /folder/fromLocal:/folder/fromDocker # Mount the folder with the container  

docker volume ls # All volumes
docker volume rm volume_name # delete a volume

```

# Delete image

```
docker rmi image_name:version/image-id  # remove single image
docker rmi $(docker images -q)  # delete all image
docker rmi $(docker images --quiet | grep -v $(docker images --quiet ubuntu:my-image)) # delete all expect ubuntu:my-image
```
