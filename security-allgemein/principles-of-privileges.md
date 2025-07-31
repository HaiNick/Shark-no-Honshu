# Principles of Privileges

Die Verwaltung von Zugriffsrechten ist zentral für die Sicherheit von IT-Systemen. Zugriffslevel für Einzelpersonen hängen hauptsächlich ab von:

* ihrer **Rolle/Funktion** in der Organisation
* der **Sensibilität** der Daten, auf die zugegriffen wird

Zwei Schlüsselkonzepte unterstützen diese Verwaltung:

* **Privileged Identity Management (PIM):**\
  <mark style="color:yellow;">**\[ ? ] PIM**</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">verwaltet</mark> <mark style="color:yellow;"></mark>_<mark style="color:yellow;">wer Zugriff bekommt</mark>_\
  Ordnet Benutzerrollen innerhalb der Organisation entsprechenden Systemrollen zu.
* **Privileged Access Management (PAM):**\
  <mark style="color:yellow;">**\[ ? ] PAM**</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">kontrolliert</mark> <mark style="color:yellow;"></mark>_<mark style="color:yellow;">wie dieser Zugriff genutzt wird</mark>_\
  Verwaltet und sichert privilegierte Zugriffe auf Systeme, einschließlich Passwortmanagement, Auditierung und Angriffsflächenreduzierung.

Ein grundlegendes Prinzip ist das **Prinzip der geringsten Privilegien**:\
Nutzer sollen nur die Zugriffsrechte erhalten, die sie **unbedingt für ihre Aufgaben benötigen** – nicht mehr.

PIM und PAM überschneiden sich in Teilen, erfüllen jedoch unterschiedliche Zwecke innerhalb der Zugriffskontrolle.

***

### Unterschied zwischen PIM und PAM

| **Aspekt**              | **PIM (Privileged Identity Management)**                               | **PAM (Privileged Access Management)**                              |
| ----------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Zweck**               | Verwaltung von Benutzerrollen und deren Zuordnung zu Systemrollen      | Verwaltung und Absicherung von privilegierten Zugriffen auf Systeme |
| **Fokus**               | _Wer_ darf was tun (Rollen und Identitäten)                            | _Wie_ der Zugriff erfolgt und wie er kontrolliert wird              |
| **Beispiele**           | Ein Entwickler bekommt temporär Adminrechte für ein Projekt            | Protokollierung von Adminzugriffen, Passwortrotation                |
| **Typische Funktionen** | Rollenbasierte Zugriffszuweisung, temporäre Rollenzuweisung            | Passwortmanagement, Session Monitoring, Audit-Logs                  |
| **Ziel**                | Sicherstellen, dass nur berechtigte Personen bestimmte Rollen bekommen | Kontrolle und Sicherung der Nutzung dieser Rollen                   |

***

### Sicherheitsmodelle: Bell-LaPadula & Biba

In der Informationssicherheit dienen formale Sicherheitsmodelle dazu, Zugriffsregeln systematisch zu definieren und umzusetzen. Zwei klassische Modelle, die in vielen sicherheitskritischen Umgebungen Anwendung finden, sind das **Bell-LaPadula-Modell** und das **Biba-Modell**. Beide verfolgen unterschiedliche Sicherheitsziele: Während Bell-LaPadula den Schutz der **Vertraulichkeit** in den Vordergrund stellt, konzentriert sich das Biba-Modell auf die **Integrität** von Informationen.

#### Bell-LaPadula-Modell (BLP)

Das Bell-LaPadula-Modell wurde in den 1970er-Jahren im Kontext militärischer Sicherheitsanforderungen entwickelt. Es ist darauf ausgelegt, **vertrauliche Informationen vor unbefugtem Zugriff** zu schützen und insbesondere den ungewollten Abfluss sensibler Daten zu verhindern.

**Wesentliche Regeln:**

* **Simple Security Property („no read up“)**\
  → Ein Benutzer darf keine Informationen lesen, die über seiner eigenen Sicherheitsstufe liegen.
* **Star Property („no write down“)**\
  → Ein Benutzer darf keine Informationen auf eine niedrigere Sicherheitsstufe schreiben.

Diese beiden Regeln sorgen dafür, dass Informationen nicht versehentlich oder absichtlich an Benutzer mit niedrigerem Berechtigungsniveau weitergegeben werden können.

> **Einsatzgebiet:** vor allem in Bereichen mit stark klassifizierten Daten, z. B. Verteidigung, Geheimdienste, staatliche Einrichtungen.

#### Biba-Modell

Im Gegensatz dazu wurde das **Biba-Modell** mit dem Ziel entwickelt, die **Integrität von Informationen** zu gewährleisten. Es verhindert, dass Daten durch weniger vertrauenswürdige Quellen verfälscht oder kompromittiert werden.

**Wesentliche Regeln:**

* <mark style="color:yellow;">**Simple Integrity Property („no read down“)**</mark>\ <mark style="color:yellow;">→</mark> Benutzer dürfen keine Daten mit niedrigerer Integritätsstufe lesen.
* <mark style="color:yellow;">**Star Integrity Property („no write up“)**</mark>\ <mark style="color:yellow;">→</mark> Benutzer dürfen keine Daten auf einer höheren Integritätsstufe überschreiben.

Diese Regeln schützen vor einer ungewollten Beeinflussung sensibler oder besonders zuverlässiger Daten durch unzuverlässige Eingaben oder Akteure.

> **Einsatzgebiet:** insbesondere in Finanzsystemen, industriellen Steuerungen oder anderen Bereichen, in denen **Zuverlässigkeit und Korrektheit der Daten** oberste Priorität haben.

***

#### Gegenüberstellung der Modelle

| **Kriterium**            | **Bell-LaPadula**                                        | **Biba**                                                       |
| ------------------------ | -------------------------------------------------------- | -------------------------------------------------------------- |
| Schutzfokus              | Vertraulichkeit                                          | Integrität                                                     |
| Lesezugriff              | Keine Einsicht in höher eingestufte Daten („no read up“) | Keine Einsicht in niedriger eingestufte Daten („no read down“) |
| Schreibzugriff           | Kein Schreiben auf niedrigeres Niveau („no write down“)  | Kein Schreiben auf höheres Niveau („no write up“)              |
| Typischer Einsatzbereich | Militärische und behördliche Systeme                     | Wirtschaft, Industrie, kritische Infrastrukturen               |
| Schwächen                | Keine Integritätskontrolle                               | Kein Schutz der Vertraulichkeit                                |
