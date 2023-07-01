---
title: Basic Setup
subtitle: In a few basic commands you can configure and start your Gemfast server. For more details see the configuration reference.
author: greg
tags: [setup]
---

### Configuring The Gemfast Service

The Gemfast configuration file uses the [hcl](https://github.com/hashicorp/hcl) format. By default it is located at `/etc/gemfast/gemfast.hcl`.

The default configuration is empty because Gemfast uses sensible defaults as much as possible. If you start the service at this point it will start Gemfast in an unlicensed mode which makes a subset of features available. See [Free Version]() for more info.

If you have purchased a license, you can go ahead and add that to the configuration file and configure an auth mode by updating `/etc/gemfast/gemfast.hcl`:

```terraform
license_key = "<your license key>"

auth "local" {
    admin_password = "mysupersecretpassword"
}
```

For the complete configuration reference see [Configuring Gemfast]().

### Starting The Gemfast Serice

To run the following to start the gemfast server now and on startup:

```bash
sudo systemctl enable gemfast.service --now
```