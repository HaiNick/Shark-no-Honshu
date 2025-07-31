# Docker

**Container Übersicht & Management**

```
docker ps                         # laufende Container  
docker ps -a                      # alle Container  
docker images                     # verfügbare Images  
docker rm ID                      # Container löschen  
docker rmi IMAGE                  # Image löschen  
docker stop ID                    # Container stoppen  
docker start ID                   # Container starten  
docker restart ID                 # Container neu starten  
```

**Container starten**

```
docker run -it IMAGE              # interaktiv starten  
docker run -d IMAGE               # detached starten  
docker run --rm IMAGE             # starten und nach Beenden löschen  
docker run --name NAME IMAGE      # benannten Container starten  
```

**In Container arbeiten**

```
docker exec -it ID bash           # Bash im Container (wenn vorhanden)  
docker exec -it ID sh             # SH im Container (leichter verfügbar)  
docker attach ID                  # an Prozess anhängen  
```

**Dateien kopieren**

```
docker cp ID:/pfad/datei .         # Datei aus Container holen (Download) 
docker cp ./datei ID:/pfad/        # Datei in Container legen  (Upload)
docker cp ID:/quelle ./ziel        # Verzeichnis aus Container kopieren  
docker cp ./verzeichnis ID:/ziel/  # Verzeichnis in Container kopieren   
```

**Netzwerkzugriff & Verbindungen**

```
docker network ls                 # Netzwerke anzeigen  
docker inspect ID                 # Details (inkl. IP) anzeigen  
docker run --network host IMAGE   # Host-Netzwerk verwenden  
docker run --network container:ID IMAGE  # Netzwerk eines Containers nutzen  
```

**Image bauen & verwalten**

```
docker build -t name .            # Image bauen  
docker pull image                 # Image aus Registry holen  
docker push image                 # Image in Registry laden  
```

**Systempflege**

```
docker system prune               # ungenutzte Daten löschen  
docker volume ls / rm / prune     # Volumes verwalten  
docker logs ID                    # Logs anzeigen  
```

**Container Info**

```
docker inspect ID                 # JSON-Daten zum Container  
docker top ID                     # Prozesse im Container  
docker stats                      # Container-Ressourcen live  
```
