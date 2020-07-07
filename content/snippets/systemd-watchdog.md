---
title: "Systemd Watchdog"
draft: false
---

```bash
# copy this file to /etc/systemd/system/my-service-watchdog.service

[Unit]
Description=my-service watchdog

[Service]
ExecStart=/usr/local/bin/service-watchdog.sh
Environment=FRAGILE_SERVICE=my-service TIMEOUT=5m
Type=oneshot

# Logging
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=my-service-watchdog
SyslogLevel=info

# Sandboxing
ProtectSystem=strict
PrivateDevices=yes
PrivateTmp=yes
NoNewPrivileges=yes

[Install]
WantedBy=multi-user.target
```

```bash
# copy this file to /etc/systemd/system/my-service-watchdog.timer
# filename must match .service file (above)

[Unit]
Description=my-service watchdog timer

[Timer]
OnCalendar=*:0/5:00
Persistent=True

[Install]
WantedBy=timers.target
```

```bash
#!/bin/bash
# copy this script to /usr/local/bin/service-watchdog.sh

TIMEOUT=${TIMEOUT:-5m}
MIN_LINES=${MIN_LINES:1}

if [ "$FRAGILE_SERVICE" = "" ]; then
  echo "FRAGILE_SERVICE is empty, aborting"
  exit 1
fi

log_lines="$(/bin/journalctl -q -u ${FRAGILE_SERVICE}.service --since "-$TIMEOUT" | /usr/bin/wc -l)"
echo "$FRAGILE_SERVICE log_lines past ${TIMEOUT}: $log_lines"

if [[ "$log_lines" -lt "$MIN_LINES" ]]; then
  echo systemctl restart ${FRAGILE_SERVICE}.service
else
  echo "$FRAGILE_SERVICE watchdog reset"
fi
```
