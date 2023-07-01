---
title: Configuring Gemfast
subtitle: This document covers the configuration options of Gemfast in depth.
author: greg
tags: [setup]
---

- [File Locations](#file-locations)
- [Configuration](#configuration)
  - [Server](#server)
  - [Caddy](#caddy)
  - [Mirror](#mirror)
  - [Filter](#filter)
  - [CVE](#cve)


### File Locations

Gemfast has a few different configuration files that it uses. The locations for these files are:

* The service configuration file: `/etc/gemfast/gemfast.hcl`
* The ACL configuration file: `/opt/gemfast/etc/gemfast/gemfast_acl.csv`
* The casbin auth_model configuration file: `/opt/gemfast/etc/gemfast/auth_model.conf`

Most users will only need to interact with the service configuration file.

### Configuration

#### Server

Gemfast server configuration applies to the Gemfast server which receives requests proxied by the caddy web server.

| Name | Description | Default |
| ---  | ----------- | ------- |
| license_key | License key purchased from gemfast.io | `nil` |
| port | Port the gemfast server listens on | 2020 |
| log_level | Log level for the gemfast server | `info` |
| dir | Base directory for gemfast data | `/var/gemfast` |
| gem_dir | Directory where gems are stored | `/var/gemfast/gems` |
| db_dir | Directory where the database file is stored | `/var/gemfast/db` |
| acl_path | Path to the acl configuration file | `/opt/gemfast/etc/gemfast/gemfast_acl.csv` |
| auth_model_path | Path to the auth_model configuration file | `/opt/gemfast/etc/gemfast/auth_model.conf` |

Configured in `/etc/gemfast/gemfast.hcl`

```terraform
license_key     = ""
port            = 2020
log_level       = "info"
dir             = "/var/gemfast"
gem_dir         = "/var/gemfast/gems"
db_dir          = "/var/gemfast/db"
acl_path        = "/opt/gemfast/etc/gemfast/gemfast_acl.csv"
auth_model_path = "/opt/gemfast/etc/gemfast/auth_model.conf"
```

#### Caddy

Caddy configuration applies to the caddy web server which is used as a reverse proxy with automatic HTTPS.

| Name | Description | Default |
| ---  | ----------- | ------- |
| port | Port caddy will listen on | `443` |
| host | The hostname for the gemfast service | `https://localhost` |
| metrics_disabled | Disable caddy metrics | `false` |
| admin_api_enabled | Enable the caddy admin API | `false` |

Configured in `/etc/gemfast/gemfast.hcl`

```terraform
caddy {
  port              = 443
  host              = "https://localhost"
  metrics_disabled  = false
  admin_api_enabled = false
}
```

#### Mirror

Mirror configuration enables a gem mirror upstream that downloads and caches gems from an upstream rubygems server.

| Name | Description | Default |
| ---  | ----------- | ------- |
| enabled | Enable or disabled mirroing | `true` |
| upstream | The upstream server to mirror | `https://rubygems.org` |

Configured in `/etc/gemfast/gemfast.hcl`

```terraform
mirror {
  enabled  = true
  upstream = "https://rubygems.org"
}
```

#### Filter

Filter configuration enables the ability to allow-list or deny-list gems from being uploaded to or downloaded by the Gemfast server. It works by matching an array of regular expressions against the name of a `.gem` file.

| Name | Description | Default |
| ---  | ----------- | ------- |
| enabled | Enable or disabled gem filtering | `true` |
| action | The action to take when a regex is matched. Values: `allow | deny` | `deny` |
| regex | Array of regular expressions to match against a gem name | `[]` |

Configured in `/etc/gemfast/gemfast.hcl`

```terraform
filter {
  enabled = false
  action  = "deny"
  regex   = []
}
```

#### CVE

CVE settings enable the ability to block gems from being downloaded or uploaded if they have a registered CVE of a certain severity. The CVE database used is stored on disk as a git repository and updated automatically in the background by the Gemfast service.

| Name | Description | Default |
| ---  | ----------- | ------- |
| enabled | Enable or disabled gem filtering based on CVE severity | `true` |
| max_severity | The action to take when a regex is matched. Values: `low | medium | high ` | `high` |
| ruby_advisory_db_dir | Directory where the ruby advisory db is stored | `/opt/gemfast/share/gemfast` |

Configured in `/etc/gemfast/gemfast.hcl`

```terraform
cve {
  enabled              = false
  max_severity         = "high"
  ruby_advisory_db_dir = "/opt/gemfast/share/gemfast"
}
```