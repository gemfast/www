---
title: Basic Setup
subtitle: Start a Gemfast server with a few basic commands and configurations. For more detailed configuration see "Configuring Gemfast".
author: greg
tags: [setup, featured]
order: 2
---

The Gemfast configuration file uses [hcl](https://github.com/hashicorp/hcl). By default it is located at `/etc/gemfast/gemfast.hcl`. The default configuration is commented out because Gemfast uses sensible defaults as much as possible. 

### Auth Modes

Gemfast includes 3 different auth modes - none, local and GitHub. The easist way to enable auth is by using local mode. With local mode you can add and remove users directly in the Gemfast configuration file. See the [configuration page](../configuration) for details

```terraform
auth "local" {
    user {
        username = "bobvance"
        password = "passw0rd"
        role     = "write"
    }
}
```

### Mirroring

The default configuration will mirror rubygems.org automatically but you can mirror any gem server using the following:

```terraform
mirror "https://gems.acme.com" {}
```

For the complete configuration reference see [Configuring Gemfast](/docs/configuration).

### Starting The Gemfast Serice

The debian package installation uses `systemd` to manage Caddy and the Gemfast server. Run the following to start the servers now and on system startup:

```bash
sudo systemctl enable gemfast.service --now
```

Check the [docker page](../docker) If using the docker image.