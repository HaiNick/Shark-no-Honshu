# investigate linux systems for incident response. example: find persistence, trace attacks, collect evidence because attackers leave digital footprints

## system identification and basic info
```bash
# system details
cat /etc/os-release            # os version and details
uname -a                       # kernel and architecture
hostname                       # system hostname
uptime                         # system uptime
cat /proc/version              # detailed kernel info
lscpu                          # cpu information
free -h                        # memory usage
df -h                          # disk usage
```

## user account analysis
```bash
# user enumeration
cat /etc/passwd                # all user accounts
cat /etc/passwd | grep -v nologin | cut -d: -f1  # real users only
cat /etc/group                 # group memberships
cat /etc/shadow                # password hashes (root only)
last                          # login history
lastlog                       # last login per user
w                             # currently logged in users
who                           # current sessions
users                         # simple user list
```

## authentication and access logs
```bash
# authentication logs
cat /var/log/auth.log          # debian/ubuntu auth events
cat /var/log/secure            # rhel/centos auth events
grep "authentication failure" /var/log/auth.log
grep "Failed password" /var/log/auth.log
grep "sudo:" /var/log/auth.log # sudo usage
grep "su:" /var/log/auth.log   # su usage

# ssh analysis
grep "ssh" /var/log/auth.log   # ssh connections
grep "Accepted" /var/log/auth.log  # successful logins
grep "Failed" /var/log/auth.log    # failed attempts
```

## process and service analysis
```bash
# running processes
ps aux                         # all processes detailed
ps -eo pid,ppid,user,args      # custom process format
pstree                         # process tree
top                           # real-time process monitor

# services
systemctl list-units --type=service --state=running  # active services
systemctl list-units --type=service --state=failed   # failed services
ls -la /etc/systemd/system/    # systemd service files
ls -la /etc/init.d/            # sysv init scripts
```

## network connections and activity
```bash
# current connections
netstat -antup                 # all connections with processes
ss -antup                      # modern netstat alternative
lsof -i                        # open network files
lsof -i :22                    # specific port

# network configuration
ip addr show                   # ip addresses
ip route show                  # routing table
cat /etc/hosts                 # host file entries
cat /etc/resolv.conf           # dns settings
arp -a                         # arp table
```

## file system forensics
```bash
# recently modified files
find / -mtime -1 -type f 2>/dev/null  # modified last 24h
find /home -mtime -7 -type f 2>/dev/null  # modified last week
find /tmp -type f -ls            # temp files with details
find /var/tmp -type f -ls        # var temp files

# unusual file locations
find / -name ".*" -type f 2>/dev/null  # hidden files
find /dev -type f 2>/dev/null          # files in /dev
find /proc -name "*.so" 2>/dev/null    # libraries in /proc

# suid/sgid files
find / -perm -4000 -type f 2>/dev/null  # suid files
find / -perm -2000 -type f 2>/dev/null  # sgid files
find / -perm -6000 -type f 2>/dev/null  # both suid and sgid

# world-writable files
find / -perm -002 -type f 2>/dev/null   # world-writable files
find / -perm -002 -type d 2>/dev/null   # world-writable dirs
```

## persistence mechanism hunting
```bash
# cron jobs
cat /etc/crontab               # system cron
ls -la /etc/cron.*             # cron directories
crontab -l -u root             # root cron
for user in $(cat /etc/passwd | cut -d: -f1); do echo "=== $user ==="; crontab -l -u $user 2>/dev/null; done

# startup scripts
ls -la /etc/init.d/            # init scripts
ls -la /etc/systemd/system/    # systemd services
systemctl list-unit-files --type=service  # all service files

# user startup files
find /home -name ".bashrc" -o -name ".bash_profile" -o -name ".profile" 2>/dev/null
cat /home/*/.bashrc 2>/dev/null | grep -v "^#" | grep -v "^$"

# system-wide startup
cat /etc/bash.bashrc           # system bash config
cat /etc/profile               # system profile
ls -la /etc/profile.d/         # profile scripts
```

## log analysis and timeline
```bash
# system logs
cat /var/log/syslog            # system events
cat /var/log/messages          # system messages (rhel/centos)
cat /var/log/kern.log          # kernel messages
tail -f /var/log/syslog        # real-time monitoring

# application logs
ls -la /var/log/               # all log files
find /var/log -name "*.log" -type f  # find log files
cat /var/log/apache2/access.log  # web server logs
cat /var/log/nginx/access.log    # nginx logs

# log parsing examples
grep "$(date +'%b %d')" /var/log/syslog  # today's logs
awk '/Jan 15/ && /error/' /var/log/syslog  # specific date and pattern
```

## command history analysis
```bash
# bash history
cat ~/.bash_history            # current user history
find /home -name ".bash_history" 2>/dev/null -exec cat {} \;  # all user histories
history                        # current session history

# other shell histories
cat ~/.zsh_history 2>/dev/null
cat ~/.fish_history 2>/dev/null

# editor histories
cat ~/.viminfo 2>/dev/null     # vim usage history
cat ~/.nano_history 2>/dev/null  # nano history
```

## ssh and key analysis
```bash
# ssh configuration and logs
cat /etc/ssh/sshd_config       # ssh daemon config
ls -la ~/.ssh/                 # user ssh directory
cat ~/.ssh/authorized_keys     # authorized public keys
cat ~/.ssh/known_hosts         # known host keys

# find ssh keys
find / -name "id_rsa" -o -name "id_dsa" -o -name "id_ecdsa" 2>/dev/null
find / -name "*.pub" 2>/dev/null  # public keys
find /home -name "authorized_keys" 2>/dev/null
```

## malware and backdoor hunting
```bash
# unusual processes
ps aux | grep -v "^\[" | sort  # non-kernel processes
ps aux | awk '{print $11}' | sort | uniq -c | sort -rn  # process frequency

# network backdoors
netstat -antup | grep LISTEN   # listening services
lsof -i | grep LISTEN          # listening processes
ss -tulpn                      # listening sockets

# unusual binaries
find /tmp -type f -executable 2>/dev/null  # executables in temp
find /dev/shm -type f 2>/dev/null          # files in shared memory
find /var/tmp -type f -executable 2>/dev/null
```

## system configuration analysis
```bash
# kernel modules
lsmod                          # loaded modules
cat /proc/modules              # module details
find /lib/modules/$(uname -r) -name "*.ko"  # available modules

# system calls and traces
strace -p PID                  # trace system calls of process
ltrace -p PID                  # trace library calls

# environment variables
env                            # current environment
cat /proc/PID/environ          # process environment (replace PID)
```

## timeline reconstruction
```bash
# file timestamps
stat filename                 # detailed file timestamps
find / -type f -printf "%T@ %Tc %p\n" 2>/dev/null | sort -n  # all files by time

# log correlation
grep -h "$(date +'%b %d')" /var/log/auth.log /var/log/syslog | sort

# comprehensive timeline
find /var/log -name "*.log" -exec grep -l "$(date +'%b %d')" {} \; | xargs grep "$(date +'%b %d')" | sort
```

## evidence collection
```bash
# create forensic image
dd if=/dev/sda of=/mnt/evidence/system.img bs=4096  # [!] full disk image
dd if=/dev/sda1 of=/mnt/evidence/root.img bs=4096   # root partition

# collect volatile data
ps aux > /tmp/processes.txt
netstat -antup > /tmp/network.txt
lsof > /tmp/openfiles.txt
mount > /tmp/mounts.txt
cat /proc/meminfo > /tmp/memory.txt

# collect logs safely
cp -r /var/log /tmp/evidence/
tar czf evidence_$(date +%Y%m%d_%H%M%S).tar.gz /tmp/evidence/
```

## quick incident triage
```bash
# rapid compromise check
last | head -20                # recent logins
ps aux | grep -E "(nc|ncat|socat|perl|python|bash)" | grep -v grep
netstat -antup | grep -E ":(22|23|80|443|4444|8080)"
find /tmp /var/tmp /dev/shm -type f -newer /bin/bash 2>/dev/null
crontab -l; cat /etc/crontab   # check for persistence
```

# TODO: add memory forensics, rootkit detection, log tampering analysis, ioc extraction