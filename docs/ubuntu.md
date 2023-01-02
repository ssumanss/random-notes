---
layout: default
title: Ubuntu
nav_order: 4
---
**Step 1:** Pull a ubutu image from docker

```shell
docker run -it --name relate -p 8010:8000 ubuntu
```

**Step 2:** Update the ubuntu image

```shell
apt update && apt install -y
```

**Step 3:** Installing basic package

```shell
apt install sudo nano
```

**Step 4:** Creating new user

```bash
adduser <username>
```

**Step 5:** To login into the user

```bash
su -l <user>
```
