---
title: Backup and Restore
subtitle: This document covers creating backups and restoring from a backup
author:
tags: [features]
---

### Creating Backups

There are two directories to backup Gemfast:

* `/var/gemfast` - all data including gems, database, etc.
* `/etc/gemfast` - configuration data 

It is recommended to create periodic backups by compressing and moving these two folders to an off-server location like Amazon S3 or similar.

For example:

```bash
sudo tar czf "gemfast_backup_$(date +%s).tar.gz" /var/gemfast /etc/gemfast
```

### Restoring From a Backup

To restore from a backup, unpack the backed up folders to their original locations and start up the Gemfast server.

```bash
tar xvf gemfast_backup_1689105521.tar.gz
sudo mv ./var/gemfast /var/
sudo mv ./etc/gemfast /etc/
sudo chmod -R gemfast:gemfast /var/gemfast
sudo chmod -R gemfast:gemfast /etc/gemfast
sudo systemctl restart gemfast.service
```