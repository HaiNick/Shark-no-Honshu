# linux system administration commands. example: system info, process control, network tools because you need to manage systems

## system information
```bash
# basic system details
uname -a                           # kernel and system info
cat /etc/os-release                # distribution info
cat /etc/*-release                 # alternative release info
lsb_release -a                     # detailed release info
hostnamectl                        # systemd host info
cat /proc/version                  # kernel version details

# hardware information
lscat                              # hardware information
lsusb                              # usb devices
lsblk                              # block devices
df -h                              # disk usage
free -h                            # memory usage
cat /proc/cpuinfo                  # cpu details
cat /proc/meminfo                  # memory details
```

## process and service management
```bash
# processes
ps aux                             # all processes
ps aux | grep process_name         # find specific process
pgrep process_name                 # get pid by name
pkill process_name                 # kill by name
kill -9 PID                        # force kill process
killall process_name               # kill all instances
jobs                               # background jobs
bg                                 # background last job
fg                                 # foreground last job

# services (systemd)
systemctl status service_name      # service status
systemctl start service_name       # start service
systemctl stop service_name        # stop service
systemctl restart service_name     # restart service
systemctl enable service_name      # enable at boot
systemctl disable service_name     # disable at boot
systemctl list-units --type=service  # list all services
journalctl -u service_name         # service logs
```

## file and directory operations
```bash
# find files
find / -name "filename" 2>/dev/null  # find by name
find / -type f -size +100M 2>/dev/null  # files larger than 100MB
find / -perm -4000 2>/dev/null     # suid files
find / -user username 2>/dev/null  # files owned by user
locate filename                    # fast file search (needs updatedb)

# file permissions
chmod 755 file                     # set permissions
chown user:group file              # change ownership
chgrp group file                   # change group
umask                              # default permissions mask
```

## network commands
```bash
# network configuration
ip addr show                       # ip addresses
ip route show                      # routing table
ifconfig                           # interface configuration (legacy)
netstat -tulpn                     # listening ports
ss -tulpn                          # modern netstat alternative
arp -a                             # arp table

# network testing
ping host                          # test connectivity
traceroute host                    # trace route
nslookup host                      # dns lookup
dig host                           # detailed dns query
wget url                           # download file
curl url                           # http request

# network scanning
nmap target                        # basic port scan
nmap -sS target                    # stealth syn scan
nmap -sV target                    # version detection
nmap -A target                     # aggressive scan
nmap -p- target                    # all ports
```

## monitoring and performance
```bash
# system monitoring
top                                # process monitor
htop                               # better process monitor
iotop                              # io monitoring
nethogs                            # network usage by process
iftop                              # network interface monitoring
vmstat                             # virtual memory stats
iostat                             # io statistics

# disk usage
du -sh directory                   # directory size
du -sh * | sort -h                 # sorted directory sizes
ncdu /                             # interactive disk usage
```

## archive and compression
```bash
# tar operations
tar -czf archive.tar.gz files      # create compressed archive
tar -xzf archive.tar.gz            # extract compressed archive
tar -tf archive.tar.gz             # list archive contents
tar -czf backup.tar.gz --exclude=*.log /home  # exclude patterns

# other compression
zip -r archive.zip directory       # create zip
unzip archive.zip                  # extract zip
gzip file                          # compress file
gunzip file.gz                     # decompress file
```

## text processing
```bash
# quick text operations
cat file                           # display file
less file                          # page through file
head -n 10 file                    # first 10 lines
tail -n 10 file                    # last 10 lines
tail -f file                       # follow file (real-time)
grep "pattern" file                # search in file
wc -l file                         # count lines
sort file                          # sort lines
uniq file                          # remove duplicates
```

## user and group management
```bash
# user operations
whoami                             # current user
id                                 # user and group ids
w                                  # logged in users
last                               # login history
passwd                             # change password
su - username                      # switch user
sudo command                       # run as root

# group operations
groups                             # show user groups
groups username                    # show user's groups
```

## package management
```bash
# debian/ubuntu (apt)
apt update                         # update package list
apt upgrade                        # upgrade packages
apt install package                # install package
apt remove package                 # remove package
apt search package                 # search packages

# rhel/centos (yum/dnf)
yum update                         # update packages
yum install package                # install package
yum remove package                 # remove package
dnf install package                # newer package manager

# arch (pacman)
pacman -Syu                        # update system
pacman -S package                  # install package
pacman -R package                  # remove package
```

## cron and scheduling
```bash
# cron management
crontab -l                         # list cron jobs
crontab -e                         # edit cron jobs
crontab -r                         # remove all cron jobs
cat /etc/crontab                   # system cron table

# at command (one-time scheduling)
at 15:30                           # schedule command at time
atq                                # list scheduled jobs
atrm job_number                    # remove scheduled job
```

## quick one-liners
```bash
# system overview
ps aux --sort=-%cpu | head         # top cpu processes
du -sh /var/log/* | sort -h        # log file sizes
netstat -tulpn | grep :80          # what's on port 80
find /tmp -type f -mtime +7 -delete  # delete old temp files
history | tail -50                 # recent commands
```

# TODO: add more advanced tools documentation, systemd deep dive, performance tuning commands