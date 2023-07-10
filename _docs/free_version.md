---
title: Free Version
subtitle: Gemfast provides a subset of features for free users.
author:
tags: [setup]
---

Gemfast packages are available to users without a license via the [downloads](https://downloads.gemfast.io/gemfast-server) page. When starting the Gemfast server, an HTTP request is made to validate the license provided in the /etc/gemfast/gemfast.hcl configuration file. If the license is not provided or not valid, the server starts in the free version mode.

The free version supports uploading and downloading [Private Gems]() and [Automatic HTTPS]() via Caddy.