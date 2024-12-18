---
title: "Linux Server Docker Configuration"
date: 2024-12-18
categories: [Linux, Docker]
tags: [linux, docker]
---

# Linux Server Docker Configuration

## Main operations

### Create an image container that supports cudnn and cuda (GPU version)

```shell
nvidia-docker run -d -p <free port>:22 --privileged=true --name <container name> -v /data/<folder name>:/data --shm-size="1g" --restart=always lockens/anaconda:3.0 /usr/sbin/sshd -D
```

