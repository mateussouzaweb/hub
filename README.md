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
sudo mkdir -p /usr/share/stack/ && cd /usr/share/stack/
sudo git clone https://github.com/mateussouzaweb/hub.git hub && cd hub
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
sudo infralet run server/install
sudo infralet run ufw/install
sudo infralet run fail2ban/install
sudo infralet run tools/install

# Databases
sudo infralet run mongodb/install
sudo infralet run mysql/install
sudo infralet run postgresql/install
sudo infralet run redis/install

# Web Server
sudo infralet run nodejs/install
sudo infralet run nginx/install
sudo infralet run php/install
sudo infralet run php/composer/install
sudo infralet run php/nginx/install
sudo infralet run php/redis/install

# Reverse Proxy
sudo infralet run traefik/install

# Monitoring Server
sudo infralet run grafana/install
sudo infralet run grafana/traefik/install
sudo infralet run influxdb/install
sudo infralet run influxdb/traefik/install
sudo infralet run loki/install
sudo infralet run loki/traefik/install
sudo infralet run promtail/install

# Email Server
sudo infralet run postfix/install
sudo infralet run postfix/mysql/install
sudo infralet run postfix/autoconfig/install
sudo infralet run dovecot/install
sudo infralet run dovecot/mysql/install
sudo infralet run rspamd/install
sudo infralet run rspamd/redis/install
sudo infralet run rspamd/traefik/install

# Monitoring Agent
sudo infralet run monit/install
sudo infralet run monit/traefik/install

sudo infralet run telegraf/install
sudo infralet run dovecot/telegraf/install
sudo infralet run fail2ban/telegraf/install
sudo infralet run influxdb/telegraf/install
sudo infralet run mongodb/telegraf/install
sudo infralet run monit/telegraf/install
sudo infralet run mysql/telegraf/install
sudo infralet run nginx/telegraf/install
sudo infralet run php/telegraf/install
sudo infralet run postfix/telegraf/install
sudo infralet run redis/telegraf/install
sudo infralet run traefik/telegraf/install

# Backups
sudo infralet run mongodb/dump/backup
sudo infralet run mysql/dump/backup

sudo infralet run rsync/files
sudo infralet run rsync/incremental

sudo infralet run rclone/install
sudo infralet run rclone/cdn
sudo infralet run rclone/copy
sudo infralet run rclone/sync
sudo infralet run rclone/move

sudo infralet run borg/install
sudo infralet run borg/snapshot

# Restore
sudo infralet run mysql/dump/restore
sudo infralet run mongodb/dump/restore
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
/usr/local/bin/infralet load_variables variables.env

# Run scripts
/usr/local/bin/infralet run hub/mysql/dump/backup
/usr/local/bin/infralet run hub/mongodb/dump/backup
/usr/local/bin/infralet run hub/rsync/files
/usr/local/bin/infralet run hub/borg/snapshot
```

Now, add it to the crontab:

```bash
sudo crontab -e
```

```bash
# APPEND
30 1 * * * /usr/share/stack/scripts/backup.sh
```
