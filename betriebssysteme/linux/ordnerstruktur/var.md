# /var variable data because systems generate logs and cache.

## key directories
- `/var/log`: system logs - essential for debugging/forensics
- `/var/spool`: queues - mail, print jobs, cron tasks
- `/var/cache`: temporary cache data for performance
- `/var/tmp`: persistent temp files (survive reboots)
- `/var/lib`: persistent application data
- `/var/run`: runtime data - PIDs, sockets (now `/run`)
- `/var/mail`: user email storage

## important logs
```bash
/var/log/syslog         # general system messages
/var/log/auth.log       # authentication attempts
/var/log/kern.log       # kernel messages
/var/log/apache2/       # web server logs
/var/log/mysql/         # database logs
```

## monitoring
```bash
tail -f /var/log/syslog              # follow system log
grep "Failed" /var/log/auth.log      # failed login attempts
find /var/log -name "*.log" -mtime -1    # logs modified today
du -sh /var/log/*                    # log directory sizes
```

## cleanup
```bash
journalctl --vacuum-time=7d          # keep systemd logs 7 days
find /var/log -name "*.gz" -mtime +30 -delete    # old compressed logs
logrotate -f /etc/logrotate.conf     # force log rotation
```

## security locations
- `/var/spool/cron/crontabs/`: user cron jobs
- `/var/lib/dpkg/info/`: package installation info
- `/var/cache/apt/archives/`: downloaded packages

## notes
[!] `/var/log` often contains sensitive info - credentials, IPs, user activity
[!] logs can fill disk space - monitor `/var` partition size
[!] `/var/tmp` persists across reboots unlike `/tmp`