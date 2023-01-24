---
layout: default
title: Docker
nav_order: 6
---

Running a container 
```bash
docker run <IMAGE>
```

List running containers 
```bash
docker ps
```

Running shell in docker image
```bash
docker run -it python:3.7-slim /bin/bash
```

Restarting docker
```shell
docker restart <container_ID>
```

Inspecting Container 
```shell
docker exec -it <container name> /bin/bash
```

