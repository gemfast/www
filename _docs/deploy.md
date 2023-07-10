---
title: Deploying Gemfast
subtitle: This document covers the setup and options of theme feature described in the doc title
author: greg
tags: [setup, featured]
order: 5
---

### Table of Contents
{:.no_toc}
* TOC
{:toc}

### Supported Platforms

Gemfast is built and tested on the following platforms.

| Platform | Architecture | Version |
| -------- | ------------ | ------- |
| Debian | `amd64`, `arm64` | `11`, `12` |
| Ubuntu | `amd64`, `arm64` | `22.04`, `23.04`

### Recommended Hardware Requirements

Gemfast has the following hardware requirements:

* 64-bit CPU architecture
* 4 total cores (physical or virtual)
* 8 GB of RAM or more
* minimum of 10 GB of free disk space in /var

### Software Requirements

Gemfast comes pre-packaged with all requirements except systemd which is included by default on the supported platforms. Ruby is not required. 


### Automatic HTTPS

Gemfast is packaged with [Caddy](https://github.com/caddyserver/caddy) which acts as a reverse proxy and can manage HTTPS certificates automatically.

In order to leverage automatic HTTPS certificates via `Caddy`, the following is required:

* Valid domain name
* A/AAAA record pointing to the server that gemfast will be installed on
* The `caddy { host = "your.domain.com" }` setting is set to the A/AAAA record in `/etc/gemfast/gemfast.hcl`
* Port 443 is open
* Port 80 is open to the world
  * Caddy will automatically re-route requests from port 80 to port 443 once a certificate has been granted by let's encrypt

Caddy and let's encypt also support other ways to generate a valid HTTPS cerificate without opening up port 80 to the world. If you require another method, please contact support.

### Manual HTTPS

If providing your own HTTPS cerificate is required, make sure the certificate and key are in `.pem` format and placed somewhere on the server. Next set the following values in `/etc/gemfast/gemfast.hcl`:

```
caddy {
  tls_cert_file = "/path/to/cert.pem"
  tls_cert_key_file = "/path/to/key.pem"
}
```

Restart Gemfast:

```
sudo systemctl restart gemfast.service
```

### Examples

Below is an example of deploying a Gemfast server to Amazon Web Services using terraform:

* [Deploying on Amazon EC2](https://github.com/gemfast/deploy-aws-ec2)
