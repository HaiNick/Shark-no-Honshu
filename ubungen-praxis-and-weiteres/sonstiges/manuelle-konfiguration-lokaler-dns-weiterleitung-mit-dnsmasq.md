# Manuelle Konfiguration lokaler DNS-Weiterleitung mit dnsmasq

#### Ziel

Einrichten eines lokalen DNS-Forwarders mit `dnsmasq`, um DNS-Anfragen in internen Netzwerken gezielt an einen internen Nameserver weiterzuleiten. Dies ist erforderlich, wenn lokale Hostnamen (z. B. `dc01.internal.corp`) aufgelöst werden müssen, die im öffentlichen DNS nicht bekannt sind.

***

#### Voraussetzungen

* `dnsmasq` ist installiert (`sudo pacman -S dnsmasq` oder `sudo apt install dnsmasq`)
* Exklusive Kontrolle über DNS-Konfiguration
* IP-Adresse des internen DNS-Servers bekannt, z. B. `10.20.30.10` (Domain Controller oder interner BIND-Server)

***

#### Konfigurationsschritte

**1. Konfigurationsdatei erstellen**

```bash
sudo nano /etc/dnsmasq.conf
```

Beispielkonfiguration:

```ini
no-resolv
resolv-file=/etc/resolv-dnsmasq
listen-address=127.0.0.1
```

**2. Interne DNS-Weiterleitung definieren**

```bash
echo "nameserver 10.20.30.10" | sudo tee /etc/resolv-dnsmasq
```

> Ersetze `10.20.30.10` durch die tatsächliche IP-Adresse des internen DNS-Servers.

**3. Mögliche Konflikte durch systemd-resolved beheben**

Falls aktiv:

```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
sudo rm -f /etc/resolv.conf
sudo ln -s /run/dnsmasq/resolv.conf /etc/resolv.conf  # optional, je nach Distribution
```

**4. Dienst aktivieren und starten**

```bash
sudo systemctl enable --now dnsmasq
```

***

#### Test der Konfiguration

DNS-Auflösung über localhost prüfen:

```bash
nslookup dc01.internal.corp 127.0.0.1
```

Oder:

```bash
dig @127.0.0.1 dc01.internal.corp
```

***

#### Optionale Systemweiterleitung

Falls das gesamte System `dnsmasq` nutzen soll:

```bash
echo "nameserver 127.0.0.1" | sudo tee /etc/resolv.conf
```

***

{% hint style="info" %}
#### Hinweise

* Die Datei `/etc/resolv-dnsmasq` wird nur berücksichtigt, wenn `no-resolv` in `dnsmasq.conf` gesetzt ist.
* Dienste wie `NetworkManager` oder `dhclient` können `/etc/resolv.conf` automatisch überschreiben.
* Falls `dnsmasq` nicht startet, prüfen mit: `sudo dnsmasq --test` und Konflikte auf Port 53 mit `sudo lsof -i :53`.
{% endhint %}

***

**Einsatzzweck:**\
Diese Konfiguration ermöglicht eine gezielte DNS-Weiterleitung in internen Netzwerken, etwa bei Active-Directory-Infrastrukturen, Segmentierungsanalysen oder Penetrationstests, in denen lokale Hostnamen und Ressourcen erreichbar sein müssen.

oder auch:

Unter BlackArch muss die DNS-Konfiguration wie folgt vorgenommen werden, sofern der NetworkManager verwendet wird:

1.  Öffnen des grafischen Konfigurationstools mit:

    ```
    nm-connection-editor
    ```
2. Auswahl der aktiven Netzwerkverbindung.
3. Wechsel zum Reiter **IPv4-Einstellungen**.
4. Auswahl der Methode: **Automatisch (DHCP)** oder **Manuell**.
5. Eintrag der gewünschten DNS-Server im Feld **DNS-Server**, z. B.:
   * IP-Adresse des THMDC gemäß Netzwerkdiagramm.
   * Zusätzlich ein öffentlicher DNS wie **1.1.1.1** für Internetzugang.
6. Speichern der Einstellungen.
7.  Neustart des NetworkManagers mit:

    ```
    sudo systemctl restart NetworkManager
    ```

Zum Testen der DNS-Auflösung kann Folgendes verwendet werden:

```bash
dig @<THMDC-IP> <Ziel-Domain>
```

oder

```bash
nslookup <Ziel-Domain> <THMDC-IP>
```
