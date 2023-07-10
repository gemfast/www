---
title: Basic Setup
subtitle: Start a Gemfast server with a few basic commands and configurations. For more detailed configuration see "Configuring Gemfast".
author: greg
tags: [setup, featured]
order: 2
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