# systemctl

### Anzeige Prozessstatus

```
systemctl status prozess.service
```

#### Neustart

```
systemctl restart prozess.service
```

#### Stoppen/Starten

```
systemctl stop/start prozess.service
```



### Auflistung fehlgeschlagener Jobs

```
systemctl --type=service --state=failed
```

```
systemctl --type=service --state=failed,exited
```

### Auflistung aktive Jobs

```
systemctl --type=service --state=running
```

```
systemctl --type=service --state=active
```

### Anzeige Unit-Files



```
systemctl list-unit-files --state=enabled
```
