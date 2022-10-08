---
layout: post
status: publish
published: true
title: 'Mongodb Docker Container, UID 999, and Ubuntu 22.04 Jammy'
categories:
- docker
- mystery
- software
tags: 
- mongodb
- docker
- conflict
- uid
comments: []
---

TLDR; Mongodb Docker containers run the mongodb uid as 999. On Ubuntu 22.04 Jammy, process 999 uid is LXD. Conflict mapping files into the mongodb container.

Working with a legacy version of [mongodb docker container](https://hub.docker.com/_/mongo/ "webpage for Mongo db container") there seems to be a problem when using Ubuntu 22.04 Jammy.

Ubuntu 22.04 has the LXD process as uid 999. Mongodb inside of the container is running as uid 999 so there is an overlap conflict as outlined in this [GitHub issue](https://github.com/docker-library/mongo/issues/181 "webpage for git hub issue"). Though under Ubuntu 20.04, systemd-coredump was running as uid 999.

The question becomes the quick hack is to just change the ownership of files that need to be mounted in the container to be owned by uid 999 (systemd-coredump or lxd) -- it works by the way.

```bash
-rw-rw-r-- 1 user1 user1  200 Sep 30 02:44 mongodb.conf
-r-------- 1 lxd   root  1024 Sep 30 19:41 keyfile
drwx------ 2 user1 user1 4096 Sep 30 02:44 mongo-express
```

What might be the security implications of this kind of mapping?
