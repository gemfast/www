---
title: Installation
subtitle: Packaged versions of Gemfast are currently available for ubuntu and debian based systems and rely on systemd for managing the service.    
author: greg
tags: [setup, featured]
order: 1
---

### Install Gemfast on Debian/Ubuntu

To install Gemfast, [download the latest Debian installer from GitHhub](https://github.com/gemfast/server/releases/latest).

And then install it using `dpkg`:

```bash
sudo dpkg -i ./gemfast_<version>_amd64.deb
```

Once installed the main configuration file can found at `/etc/gemfast/gemfast.hcl`.

### Managing the Gemfast Service

Gemfast is managed using the systemd init system. Gemfast consists of a web server service `/etc/systemd/system/caddy.service` and the Gemfast server service `/etc/systemd/system/gemfast.service`. These two services are dependant on each other executing a systemctl start/stop/restart on one, causes the other to do the same action.

To start the server and enable the server, run:

```bash
sudo systemctl enable gemfast.service --now
```

To restart the server, run:

```bash
sudo systemctl restart gemfast.service
```

To stop the server, run"

```bash
sudo systemctl stop gemfast.service
```

### Upgrade Gemfast on Debian/Ubuntu

To upgrade Gemfast, download a newer version:

```bash
wget https://downloads.gemfast.io/stable/latest/gemfast-latest-x86_64.deb
```

And then install it using `dpkg`:

```bash
sudo dpkg -i ./gemfast-latest-x86_64.deb
```

This will install the newer version of Gemfast, removing the older version in the process.

### Uninstall Gemfast on Debian/Ubuntu

To uninstall Gemfast, run the following command:

```bash
sudo dpkg -r gemfast
```