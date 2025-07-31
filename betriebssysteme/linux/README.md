# linux operations because servers run on it. quick reference for common tasks.

## package management
install `.deb` packages:
```bash
sudo apt install ./package.deb    # recommended - handles deps
sudo dpkg -i package.deb && sudo apt-get install -f    # manual fix deps
```

install from source (`tar.gz`):
```bash
tar -xzf package.tar.gz && cd extracted_folder
./configure && make && sudo make install    # standard build
# or run ./install.sh if available
```

## service management
remove service from `/opt/`:
```bash
sudo systemctl stop service.service
sudo systemctl disable service.service
sudo rm /etc/systemd/system/service.service
sudo systemctl daemon-reload
sudo rm -rf /opt/programname
```

manage autostart:
```bash
sudo systemctl enable/disable service_name
```

## keyboard layout
```bash
localectl list-keymaps                  # show available
localectl status                        # current config  
sudo setxkbmap -layout de               # set german
```

## java versions
```bash
sudo archlinux-java status              # list installed
sudo archlinux-java set java-17-openjdk    # switch version
```

## cleanup orphaned files
```bash
sudo find / -name '*programname*'       # find remaining files
```

## links
- privilege escalation: `../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/linux-privesc.md`
- forensics: `../../cheat-sheets/linux-forensik.md`
- pentest stuff: https://viperone.gitbook.io/pentest-everything/everything/everything-linux/linux-privilege-escalation-techniques

## notes
[!] services in `/etc/init.d/` vs `/etc/systemd/system/` - location matters
[!] `./` prefix required for local `.deb` files with `apt install`