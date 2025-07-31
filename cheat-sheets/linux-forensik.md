# Linux Forensik

## System & OS - Informationen

| Bezeichnung         | Pfad                 | Lesbarkeit                 |
| ------------------- | -------------------- | -------------------------- |
| OS Release          | `/etc/os-release`    | any                        |
| User Account        | `/etc/passwd`        | any                        |
| User Gruppen        | `/etc/group`         | any                        |
| Liste der Sudoer    | `/etc/sudoers`       | sudo/root                  |
| Login Informationen | `/var/log/wtmp`      | sudo last -f /var/log/wtmp |
| Authentication      | `/var/log/auth.log*` | any                        |

## Systemkonfiguration

<table><thead><tr><th width="296">Bezeichnung</th><th width="267">Pfad</th><th>Befehl</th></tr></thead><tbody><tr><td>Hostname</td><td><code>/etc/hostname</code></td><td></td></tr><tr><td>Zeitzone</td><td><code>/etc/timezone</code></td><td></td></tr><tr><td>Netzwerk Interfaces</td><td><code>/etc/network/interfaces</code></td><td>ip address show</td></tr><tr><td>Offene Netzwerk-Verbindungen</td><td></td><td>netstat -natp</td></tr><tr><td>Laufende Prozesse</td><td></td><td>ps aux</td></tr><tr><td>DNS Hostname Auflösung</td><td><code>/etc/hosts</code></td><td></td></tr><tr><td>DNS Server</td><td><code>/etc/resolv.conf</code></td><td></td></tr></tbody></table>

## Persistenz

| Bezeichnung               | Pfad                  | Bedeutung |
| ------------------------- | --------------------- | --------- |
| Cronjobs                  | /etc/crontab          |           |
| Registrierte Dienste      | /etc/init.d           |           |
| Bash Shell Startup : user | /home/\<user>/.bashrc |           |
| " " : systemweit          | /etc/bash.bashrc      |           |
| " " : systemweit          | /etc/profile          |           |

## Beweise für die Ausführung

| Bezeichnung           | Pfad                         | Bedeutung |
| --------------------- | ---------------------------- | --------- |
| Authentifikationslogs | /var/log/auth.log\*          |           |
| Bash Historie         | /home/\<user>/.bash\_history |           |
| Vim Historie          | /home/\<user>/.viminfo       |           |

## Logs

| Bezeichnung            | Pfad                | Bedeutung |
| ---------------------- | ------------------- | --------- |
| Systemlogs             | /var/log/syslog     |           |
| Authentifikationslogs  | /var/log/auth.log\* |           |
| Logs für Drittsoftware | /var/log            |           |

{% code overflow="wrap" %}
```

# Auth.log Sudo-Befehl von <user>cybert als USER mit COMMAND in Ordner PWD
sudo:   cybert : TTY=pts/0 ; PWD=/home/cybert ; USER=root ; COMMAND=/usr/bin/apt install dokuwiki


```
{% endcode %}
