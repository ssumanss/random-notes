
---
layout: default
title: Quickstart Sqlite
nav_order: 8
---

## A sample github action for docker image 

```
name: Docker Image CI

on:
  push:
    branches: [ "main" ]

jobs:

  build:

    runs-on: self-hosted

    steps:
    - uses: actions/checkout@v3
    - name: Build the Docker image and deploy
      run: |
        git pull --recurse-submodules
        docker-compose up --build -d 
```