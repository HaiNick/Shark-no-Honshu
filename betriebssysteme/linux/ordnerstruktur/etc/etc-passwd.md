# /etc/passwd

## Passwort zum Einfügen generieren

```
openssl passwd -1 -salt <SALZ> <PASSWD>
```

```
#Aufbau

Username, pw_hash, id, gid, home_dir, shell
root:x:0:0:root:/root:/bin/bash
```
