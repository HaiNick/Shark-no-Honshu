# Rogue LDAP-Server installieren

### Unter BlackArch installieren und konfigurieren

1. **Installation**

```bash
sudo pacman -S openldap
```

2. **Beispielkonfiguration bereitstellen**

Arch Linux stellt keine vorgefertigte `slapd.conf` zur Verfügung. Damit ein funktionierender LDAP-Server eingerichtet werden kann, muss entweder eine eigene `slapd.conf` erstellt oder direkt mit dem `cn=config`-Verzeichnis im LDIF-Format gearbeitet werden (empfohlen).

3. **Beispiel: Minimale `slapd.conf`**

Damit der Server startet, muss folgende Konfiguration unter `/etc/openldap/slapd.conf` hinterlegt werden:

```conf
include         /etc/openldap/schema/core.schema
pidfile         /run/slapd/slapd.pid
argsfile        /run/slapd/slapd.args

database        mdb
maxsize         1073741824
suffix          "dc=example,dc=com"
rootdn          "cn=admin,dc=example,dc=com"
rootpw          secret
directory       /var/lib/openldap/openldap-data

access to * by * write
```

> **Hinweis:** Die Werte für `dc=example,dc=com` sind an die Zielumgebung anzupassen, z. B. `dc=za,dc=tryhackme,dc=com`.

4. **Verzeichnisse anlegen und Berechtigungen setzen**

```bash
sudo mkdir -p /var/lib/openldap/openldap-data
sudo chown -R ldap:ldap /var/lib/openldap
```

5. **Datenbank initialisieren und Dienst starten**

```bash
sudo slaptest -f /etc/openldap/slapd.conf -F /etc/openldap/slapd.d
sudo systemctl start slapd
```

***

{% code title="Rogue LDAP-Installer (Blackarch)" %}
```bash
#!/bin/bash
# OpenLDAP Installation und Konfiguration unter BlackArch

set -e

# Variablen anpassen
DOMAIN="za.company.com"
BASEDN="dc=za,dc=company,dc=com"
ROOTDN="cn=admin,$BASEDN"
ROOTPW="secret"
LDAP_DATA_DIR="/var/lib/openldap/openldap-data"
SLAPD_CONF="/etc/openldap/slapd.conf"
SLAPD_CONF_DIR="/etc/openldap/slapd.d"

# 1. OpenLDAP Installation
sudo pacman -Syu --noconfirm openldap

# 2. Verzeichnisse anlegen und Berechtigungen setzen
sudo mkdir -p "$LDAP_DATA_DIR"
sudo chown -R ldap:ldap "$LDAP_DATA_DIR"
sudo mkdir -p "$SLAPD_CONF_DIR"
sudo chown -R ldap:ldap "$SLAPD_CONF_DIR"
sudo chmod -R 750 "$SLAPD_CONF_DIR"

# 3. slapd.conf erstellen
sudo tee "$SLAPD_CONF" > /dev/null <<EOF
include         /etc/openldap/schema/core.schema
pidfile         /run/openldap/slapd.pid
argsfile        /run/openldap/slapd.args

modulepath      /usr/lib/openldap
moduleload      back_mdb.la

database        mdb
maxsize         1073741824
suffix          "$BASEDN"
rootdn          "$ROOTDN"
rootpw          $ROOTPW
directory       $LDAP_DATA_DIR

access to * by * write
index objectClass eq

database monitor
EOF

sudo chown ldap:ldap "$SLAPD_CONF"
sudo chmod 640 "$SLAPD_CONF"

# 4. slapd.d-Konfiguration mit slaptest initialisieren
sudo slaptest -f "$SLAPD_CONF" -F "$SLAPD_CONF_DIR"

# 5. Berechtigungen für slapd.d erneut setzen (wichtig)
sudo chown -R ldap:ldap "$SLAPD_CONF_DIR"
sudo chmod -R 750 "$SLAPD_CONF_DIR"

# 6. slapd Dienst starten und aktivieren
sudo systemctl enable slapd
sudo systemctl start slapd

# Hinweis: base.ldif manuell erstellen und importieren:
echo "Bitte base.ldif mit Ihrem LDAP-Daten erstellen und mit folgendem Befehl importieren:"
echo "sudo ldapadd -x -D \"$ROOTDN\" -W -f /etc/openldap/base.ldif"

```
{% endcode %}

```bash
# Erweiterung: LDAP-Server auf PLAIN und LOGIN Authentifizierung beschränken

# 1. olcSaslSecProps.ldif anlegen
cat <<EOF | sudo tee /etc/openldap/olcSaslSecProps.ldif
dn: cn=config
replace: olcSaslSecProps
olcSaslSecProps: noanonymous,minssf=0,passcred
EOF

# 2. LDAP-Konfiguration mit der LDIF-Datei patchen und slapd neu starten
sudo ldapmodify -Y EXTERNAL -H ldapi:// -f /etc/openldap/olcSaslSecProps.ldif
sudo systemctl restart slapd

# 3. Überprüfung der unterstützten Authentifizierungsmethoden
echo "Prüfen der unterstützten SASL-Mechanismen:"
ldapsearch -H ldap:// -x -LLL -s base -b "" supportedSASLMechanisms
```

