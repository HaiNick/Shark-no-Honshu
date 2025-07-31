# systemctl service management because systemd runs everything.

## service control
```bash
systemctl status service.service         # check service status
systemctl start service.service          # start service
systemctl stop service.service           # stop service  
systemctl restart service.service        # restart service
systemctl reload service.service         # reload config without restart
```

## enable/disable services
```bash
systemctl enable service.service         # start on boot
systemctl disable service.service        # don't start on boot
systemctl is-enabled service.service     # check if enabled
```

## service investigation
```bash
systemctl --type=service --state=failed  # show failed services
systemctl --type=service --state=running # show running services
systemctl --type=service --state=active  # show active services
systemctl list-unit-files --state=enabled    # show enabled services
```

## logs and debugging
```bash
journalctl -u service.service            # show service logs
journalctl -u service.service -f         # follow logs real-time
journalctl -u service.service --since "1 hour ago"    # recent logs
systemctl daemon-reload                  # reload systemd config
systemctl reset-failed                   # clear failed state
```

## useful filters
```bash
systemctl list-units --type=service      # all service units
systemctl list-units --failed            # failed units only
systemctl list-dependencies service.service    # service dependencies
```

## notes
[!] always daemon-reload after changing service files
[!] failed services stay in failed state until reset
[!] some services require specific user context to function