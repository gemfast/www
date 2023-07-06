---
title: Installation
subtitle: Packaged versions of Gemfast are currently available for ubuntu and debian based systems and rely on systemd for managing the service.    
author: greg
tags: [setup, featured]
order: 1
---

### Install Gemfast on Ubuntu

To install Gemfast, download a `.deb` package from `downloads.gemfast.io`:

```bash
wget https://downloads.gemfast.io/stable/latest/gemfast-latest-x86_64.deb
```

And then install it using `dpkg`:
```bash
dpkg -i ./gemfast-latest-x86_64.deb
```

If using an arm64 architecture make sure to download `gemfast-latest-arm64.deb` instead.

### Upgrade Gemfast on Ubuntu

To upgrade Gemfast, download a newer version:

```bash
wget https://downloads.gemfast.io/stable/latest/gemfast-latest-x86_64.deb
```

And then install it using `dpkg`:
```bash
dpkg -i ./gemfast-latest-x86_64.deb
```

This will install the newer version of Gemfast, removing the older version in the process.

### Uninstall Gemfast on Ubuntu

To uninstall Gemfast, run the following command:

```bash
dpkg -r gemfast
```