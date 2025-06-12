---
title: "Linux Server Docker Configuration"
date: 2024-12-18
categories: [Linux, Docker]
tags: [Linux, Docker]
description: Docker configuration method for linux server.
---

## Main operations

### Create an image container that supports cudnn and cuda (GPU version)

```shell
nvidia-docker run -d -p <free port>:22 --privileged=true --name <container name> -v /data/<folder name>:/data --shm-size="1g" --restart=always lockens/anaconda:3.0 /usr/sbin/sshd -D
```

> The following items need to be replaced
> - free port
> - container name
> - folder name

### Enter the container

```shell
docker exec -it <container ID> /bin/bash
```

## Others

### Save running containers as docker images

```shell
sudo docker commit <container ID> <repository name>:<tag>
```

### Clean up files 60 days old in share RecycleBin

```shell
sudo find /nas/share/RecycleBin/ -mtime +60 -exec rm {} \;
```

### Clean up files 60 days old in data RecycleBin

```shell
sudo find /nas/data/RecycleBin/ -mtime +60 -exec rm {} \;
```