---
title: Linux Server Docker Configuration
date: 2024-12-18 16:57:00 +0800
categories: [Linux, Docker]
tags: [TAG]     # TAG names should always be lowercase
---

# Linux Server Docker Configuration

## Main operations

### Create an image container that supports cudnn and cuda (GPU version)

```shell
nvidia-docker run -d -p <free port>:22 --privileged=true --name <container name> -v /data/<folder name>:/data --shm-size="1g" --restart=always lockens/anaconda:3.0 /usr/sbin/sshd -D
```
{: .nolineno }
