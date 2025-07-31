# Strukturierung von Computerkonten

### &#x20;Ausgangssituation: Der Standard-Container „Computers“

Standardmäßig werden **alle Geräte**, die einer Active Directory-Domäne beitreten (mit Ausnahme der Domänencontroller), automatisch im Container **„Computers“** gespeichert. Dieser Container ist allerdings **keine Organisationseinheit (OU)**, sondern ein spezieller Systemcontainer, der sich nicht für Gruppenrichtlinien oder gezielte Verwaltungsmaßnahmen eignet.

Wenn wir unseren Domain Controller (DC) untersuchen, sehen wir bereits einige Geräte in diesem Container, beispielsweise:

* Server
* Laptops
* Desktop-PCs von Endbenutzern

Diese Vermischung erschwert eine sinnvolle Verwaltung, vor allem wenn verschiedene Gruppenrichtlinien auf Server und Arbeitsplatzrechner angewendet werden sollen.

***

### Best Practices: Geräte nach Funktion trennen

Eine strukturierte Unterteilung der Geräte in Active Directory sorgt für:

* **Bessere Übersicht**
* **Zielgerichtete Richtlinienanwendung**
* **Erhöhte Sicherheit durch differenzierte Rechtevergabe**

Ein bewährter Ansatz besteht darin, die Geräte je nach Nutzung in **getrennte Organisationseinheiten (OUs)** zu verschieben. Mindestens die folgenden drei Kategorien sollten gebildet werden:

**1. Workstations**

Workstations (Arbeitsplatzrechner) sind die Geräte, an denen die Benutzer ihre tägliche Arbeit erledigen. Sie dienen zur Textverarbeitung, zum Surfen oder zum Zugriff auf Unternehmensressourcen.

**Wichtige Hinweise:**

* Keine privilegierten Konten (z. B. Domain Admins) sollten sich jemals dauerhaft auf Workstations anmelden.
* Gruppenrichtlinien sollten Sicherheitsfeatures wie eingeschränkte Benutzerrechte, Softwareeinschränkungen und automatische Updates umfassen.

**2. Servers**

Server übernehmen wichtige Aufgaben im Netzwerk, z. B. als:

* Dateiserver
* Webserver
* Anwendungsserver
* Datenbankserver

**Besonderheiten:**

* Höhere Sicherheitsanforderungen
* Keine Benutzeranmeldung außerhalb der Administratoren
* Stärkere Überwachung und Logging

**3. Domain Controllers**

Domain Controller (DCs) enthalten besonders sensible Informationen wie die **gehashten Passwörter** aller Benutzerkonten in der Domäne. Windows legt für sie bereits automatisch eine dedizierte OU an:\
&#xNAN;**`Domain Controllers`**

{% hint style="warning" %}
⚠️ Diese Geräte sind **besonders schützenswert**. Zugriff sollte nur den notwendigsten Administratoren erlaubt sein.
{% endhint %}

***

### Ziel: Neue OUs für Workstations und Server erstellen

Um Ordnung in die AD-Struktur zu bringen, sollten zwei neue Organisationseinheiten direkt unter dem Domänennamen (`rmt.local`) erstellt werden:

```plaintext
psy.local
├── Domain Controllers
├── Servers
└── Workstations
```

**Vorgehen:**

1. Öffne **Active Directory-Benutzer und -Computer**
2. Rechtsklick auf die Domäne `psy.local` → **Neu → Organisationseinheit**
3. Vergib den Namen **Workstations**, dann wiederhole den Vorgang für **Servers**
4. Verschiebe die entsprechenden Geräte aus dem Container **„Computers“** in die passende neue OU:
   * Laptops und PCs → **Workstations**
   * Dienste- oder Systemserver → **Servers**

***

#### Vorteile der neuen Struktur

* **Gezielte Gruppenrichtlinienverwaltung:**\
  Unterschiedliche Sicherheits-, Update- oder Software-Richtlinien für Server vs. Clients
* **Delegation:**\
  Beispielsweise kann der Helpdesk nur die Workstations verwalten, nicht aber die Server.
* **Sicherheitsoptimierung:**\
  Kritische Geräte wie Domain Controller oder Server sind klar abgegrenzt und können stärker geschützt werden.
