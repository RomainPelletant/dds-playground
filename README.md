# DDS owernship sandbox

This repository contains docker image containing FastDDS packages and two samples to play with ownership.

## Build docker image

```shell
docker build -f docker/Dockerfile -t fastdds .
```

## Launch a container based on built docker image

Stay in the root directory of this repository.
Launch container with current folder mounted directory into /workdir inside container.

```shell
docker run -it -v .:/workdir fastdds
```

## Launch another bash with the same container

Find the container ID of the current container

```shell
docker ps
```

Launch a new bash

```shell
docker exec -it <container_id> /bin/bash
```
