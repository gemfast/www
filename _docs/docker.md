---
title: Docker
subtitle: Gemfast can also be run inside a container when it makes sense to do so
author: greg
tags: [setup, featured]
order: 1
---

### Running Gemfast as a Docker Container

It is recommended to create some directories to mount into the Gemfast container otherwise the application data will be removed along with the container.

- `/var/gemfast` is for app data
- `/etc/gemfast` is for the configuration file
- `/etc/machine-id` is only needed when using a licensed version of Gemfast

```bash
mkdir -p data
mkdir -p config
touch config/gemfast.hcl
```

Next start the container with the volumes mounted:

```bash
docker run -d --name gemfast-server -p 2020:2020 \
  -v data:/var/gemfast \
  -v config:/etc/gemfast \
  -v /etc/machine-id:/etc/machine-id \
  ghcr.io/gemfast/server:latest
```

The main difference between the debian packaged version and the docker image is that Caddy is not included in the docker image. That means Gemfast is listening on port 2020 by default and no automatic HTTPS is supported.

### Running Gemfast on Kubernetes

**TODO**