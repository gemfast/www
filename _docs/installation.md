---
title: Installation
subtitle: Packaged versions of Gemfast are currently available for ubuntu and debian based systems and rely on systemd for managing the service.    
author: greg
tags: [setup]
---

### Install Gemfast on Ubuntu

To install Gemfast, run the following commands:

```bash
wget https://downloads.gemfast.io/stable/gemfast/gemfast-latest-x86_64.deb
dpkg -i ./gemfast-latest-x86_64.deb
```

If using an arm64 architecture make sure to download `gemfast-latest-arm64.deb` instead.

### Upgrade Gemfast on Ubuntu

To upgrade Gemfast, download a newer version and install it:

```bash
wget https://downloads.gemfast.io/stable/gemfast/gemfast-latest-x86_64.deb
dpkg -i ./gemfast-latest-x86_64.deb
```

This will install the newer version of Gemfast, removing the older version in the process.

### Uninstall Gemfast on Ubuntu

To uninstall Gemfast, run the following command:

```bash
dpkg -r gemfast
```