# Infralet Hub

Personal hub with modules to infralet: \
<https://github.com/mateussouzaweb/infralet>

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

# Development
infralet run mkcert/install

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
