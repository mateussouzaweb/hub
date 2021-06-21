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
    "..."
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

# Databases
infralet run mongodb/install
infralet run mysql/install
infralet run postgresql/install
infralet run redis/install

# Web Server
infralet run nodejs/install
infralet run nginx/install
infralet run php/install
infralet run php/composer/install
infralet run php/nginx/install
infralet run php/redis/install

# Reverse Proxy
infralet run traefik/install

# Monitoring Server
infralet run grafana/install
infralet run grafana/traefik/install
infralet run influxdb/install
infralet run influxdb/traefik/install
infralet run loki/install
infralet run loki/traefik/install
infralet run promtail/install

# Email Server
infralet run postfix/install
infralet run postfix/mysql/install
infralet run postfix/autoconfig/install
infralet run dovecot/install
infralet run dovecot/mysql/install
infralet run rspamd/install
infralet run rspamd/redis/install
infralet run rspamd/traefik/install

# Monitoring Agent
infralet run monit/install
infralet run monit/traefik/install

infralet run telegraf/install
infralet run dovecot/telegraf/install
infralet run fail2ban/telegraf/install
infralet run influxdb/telegraf/install
infralet run mongodb/telegraf/install
infralet run monit/telegraf/install
infralet run mysql/telegraf/install
infralet run nginx/telegraf/install
infralet run php/telegraf/install
infralet run postfix/telegraf/install
infralet run redis/telegraf/install
infralet run traefik/telegraf/install

# Backups
infralet run mongodb/dump/backup
infralet run mysql/dump/backup

infralet run rsync/files
infralet run rsync/incremental

infralet run rclone/install
infralet run rclone/cdn
infralet run rclone/copy
infralet run rclone/sync
infralet run rclone/move

infralet run borg/install
infralet run borg/snapshot

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
