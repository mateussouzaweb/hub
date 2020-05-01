# Infralet Hub

Personal hub with modules to infralet: \
<https://github.com/mateussouzaweb/infralet>

```bash
# General
infralet server/install
infralet ufw/install
infralet fail2ban/install
infralet tools/install

# Web Server
infralet nginx/install
infralet php/install
infralet mongodb/install
infralet mysql/install
infralet redis/install

# Monitoring Server
infralet grafana/install
infralet influxdb/install
infralet loki/install

# Email Server
infralet mysql/install
infralet redis/install
infralet postfix/install
infralet dovecot/install
infralet rspamd/install

# Monitoring Agent
infralet monit/install
infralet telegraf/install
infralet promtail/install

# Reverse Proxy
infralet traefik/install

# Development
infralet mkcert/install

# Backups
infralet borg/install
infralet rclone/install

infralet rsync/files
infralet mongodb/dump/backup
infralet mysql/dump/backup
infralet borg/snapshot
infralet rclone/sync

# Restore
infralet mysql/dump/restore
infralet mongodb/dump/restore
```
