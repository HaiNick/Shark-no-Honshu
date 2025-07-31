# linux forensics — where attackers leave traces

## system and OS information

| item | path | access |
|------|------|--------|
| OS release | `/etc/os-release` | any user |
| user accounts | `/etc/passwd` | any user |
| user groups | `/etc/group` | any user |
| sudoers list | `/etc/sudoers` | sudo/root only |
| login info | `/var/log/wtmp` | `last -f /var/log/wtmp` |
| authentication | `/var/log/auth.log*` | any user |

## system configuration

| item | path | command |
|------|------|---------|
| hostname | `/etc/hostname` | |
| timezone | `/etc/timezone` | |
| network interfaces | `/etc/network/interfaces` | `ip addr show` |
| open connections | | `netstat -natp` |
| running processes | | `ps aux` |
| DNS hostname resolution | `/etc/hosts` | |
| DNS servers | `/etc/resolv.conf` | |

## persistence mechanisms

| mechanism | path | notes |
|-----------|------|-------|
| cronjobs | `/etc/crontab` | scheduled tasks |
| system services | `/etc/init.d/` | startup services |
| user bash startup | `/home/<user>/.bashrc` | per-user |
| system bash startup | `/etc/bash.bashrc` | system-wide |
| system profile | `/etc/profile` | system-wide |

## execution evidence

| evidence | path | notes |
|----------|------|-------|
| authentication logs | `/var/log/auth.log*` | login attempts, sudo usage |
| bash history | `/home/<user>/.bash_history` | command history |
| vim history | `/home/<user>/.viminfo` | file editing history |

## log locations

| log type | path | purpose |
|----------|------|---------|
| system logs | `/var/log/syslog` | general system events |
| auth logs | `/var/log/auth.log*` | authentication events |
| application logs | `/var/log/` | third-party software |

## parsing auth.log

sudo command format:
```
sudo: cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki
```

shows: user `cybert` ran `apt install dokuwiki` as `root` from `/home/cybert` on terminal `pts/0`
