# Infralet Hub

Personal hub with modules to infralet: \
<https://github.com/mateussouzaweb/infralet>

## Installation

Install ``infralet`` cli script on the machine:

```bash
sudo wget https://raw.githubusercontent.com/mateussouzaweb/infralet/master/infralet.sh -O /usr/local/bin/infralet && sudo chmod +x /usr/local/bin/infralet
```

Create ``stack`` folder and clone this repository into ``hub`` folder:

```bash
mkdir -p /usr/share/stack/ && cd /usr/share/stack/
git clone https://github.com/mateussouzaweb/hub.git hub
```

We now will extract variables. \
You should define every software that will be installed. Edit the following command and past it on you console containing only the necessary software:

```bash
SCRIPTS=(
    "hub/server/install"
    "hub/ufw/install"
    "hub/fail2ban/install"
    "hub/tools/install"
    "hub/nginx/install"
    "hub/php/install"
    "hub/php/composer/install"
    "hub/mongodb/install"
    "hub/mysql/install"
    "hub/redis/install"
    "hub/traefik/install"
    "hub/grafana/install"
    "hub/influxdb/install"
    "hub/loki/install"
    "hub/postfix/install"
    "hub/dovecot/install"
    "hub/rspamd/install"
    "hub/monit/install"
    "hub/telegraf/install"
    "hub/promtail/install"
    "hub/mkcert/install"
    "hub/borg/install"
    "hub/rclone/install"
    "hub/rsync/files"
    "hub/mongodb/dump/backup"
    "hub/mysql/dump/backup"
    "hub/borg/snapshot"
    "hub/rclone/sync"
    "hub/mysql/dump/restore"
    "hub/mongodb/dump/restore"
)
```

Once defined, we extract the variables to a file and update its values:

```bash
infralet extract ${SCRIPTS[@]} | sort | uniq | tee variables.env
vim variables.env
```

To finish, just run the scripts for the desired softwares:

```bash
# General
infralet run hub/server/install
infralet run hub/ufw/install
infralet run hub/fail2ban/install
infralet run hub/tools/install

# Web Server
infralet run hub/nginx/install
infralet run hub/php/install
infralet run hub/php/composer/install
infralet run hub/mongodb/install
infralet run hub/mysql/install
infralet run hub/redis/install

# Reverse Proxy
infralet run hub/traefik/install

# Monitoring Server
infralet run hub/grafana/install
infralet run hub/influxdb/install
infralet run hub/loki/install

# Email Server
infralet run hub/mysql/install
infralet run hub/redis/install
infralet run hub/postfix/install
infralet run hub/dovecot/install
infralet run hub/rspamd/install

# Monitoring Agent
infralet run hub/monit/install
infralet run hub/telegraf/install
infralet run hub/promtail/install

# Development
infralet run hub/mkcert/install

# Backups
infralet run hub/borg/install
infralet run hub/rclone/install

infralet run hub/rsync/files
infralet run hub/mongodb/dump/backup
infralet run hub/mysql/dump/backup
infralet run hub/borg/snapshot
infralet run hub/rclone/sync

# Restore
infralet run hub/mysql/dump/restore
infralet run hub/mongodb/dump/restore
```

## Automated Tasks

Some infralet modules allow running automatic tasks to make you work easy. You can for example, write a backup script with the following content and add it to crontab to be run each night:

```bash
mkdir scripts && cd scripts/
touch backup.sh && chmod +x backup.sh
vim backup.sh
```

```bash
#!/usr/bin/env bash

# Go to the defined stack directory
cd /usr/share/stack/

# Run scripts
/usr/local/bin/infralet run hub/mysql/dump/backup
/usr/local/bin/infralet run hub/mongodb/dump/backup
/usr/local/bin/infralet run hub/rsync/files
/usr/local/bin/infralet run hub/borg/snapshot
```

Now, add it to the crontab:

```bash
crontab -e
```

```bash
# APPEND
30 1 * * * /usr/share/stack/scripts/backup.sh
```
