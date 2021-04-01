# Infralet Hub

Personal hub with modules to infralet: \
<https://github.com/mateussouzaweb/infralet>

## Installation

Install ``infralet`` cli script on the machine:

```bash
sudo wget https://raw.githubusercontent.com/mateussouzaweb/infralet/master/infralet.sh -O /usr/local/bin/infralet && sudo chmod +x /usr/local/bin/infralet
```

Clone this repository into ``hub`` folder:

```bash
mkdir -p /usr/share/stack/ && cd /usr/share/stack/
git clone https://github.com/mateussouzaweb/hub.git hub && cd hub
```

We now will extract variables. \
You should define every software that will be installed. Edit the following command and past it on you console containing only the necessary software:

```bash
SCRIPTS=(
    "server/install"
    "ufw/install"
    "fail2ban/install"
    "tools/install"
    "nginx/install"
    "php/install"
    "php/composer/install"
    "mongodb/install"
    "mysql/install"
    "redis/install"
    "traefik/install"
    "grafana/install"
    "influxdb/install"
    "loki/install"
    "postfix/install"
    "dovecot/install"
    "rspamd/install"
    "monit/install"
    "telegraf/install"
    "promtail/install"
    "borg/install"
    "rclone/install"
    "rsync/files"
    "mongodb/dump/backup"
    "mysql/dump/backup"
    "borg/snapshot"
    "rclone/sync"
    "mysql/dump/restore"
    "mongodb/dump/restore"
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
infralet run server/install
infralet run ufw/install
infralet run fail2ban/install
infralet run tools/install

# Web Server
infralet run nginx/install
infralet run php/install
infralet run php/composer/install
infralet run mongodb/install
infralet run mysql/install
infralet run redis/install

# Reverse Proxy
infralet run traefik/install

# Monitoring Server
infralet run grafana/install
infralet run influxdb/install
infralet run loki/install

# Email Server
infralet run mysql/install
infralet run redis/install
infralet run postfix/install
infralet run dovecot/install
infralet run rspamd/install

# Monitoring Agent
infralet run monit/install
infralet run telegraf/install
infralet run promtail/install

# Backups
infralet run borg/install
infralet run rclone/install

infralet run rsync/files
infralet run mongodb/dump/backup
infralet run mysql/dump/backup
infralet run borg/snapshot
infralet run rclone/sync

# Restore
infralet run mysql/dump/restore
infralet run mongodb/dump/restore
```

## Automated Tasks

Some infralet modules allow running automatic tasks to make you work easy. You can for example, write a backup script with the following content and add it to crontab to be run each night:

```bash
cd /usr/share/stack/
mkdir scripts && cd scripts/

touch backup.sh && chmod +x backup.sh
vim backup.sh
```

```bash
#!/usr/bin/env bash

# Go to the defined stack directory
cd /usr/share/stack/

# Load variables
/usr/local/bin/infralet load_variables scripts/variables.env

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
