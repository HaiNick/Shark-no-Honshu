# Trees, Forests und Trusts

### Einleitung

Bisher haben wir betrachtet, wie ein einzelner Domain Controller Benutzer, Computer und Server innerhalb einer einzigen **Domain** verwaltet. Doch sobald ein Unternehmen wächst – durch Expansionen, Übernahmen oder organisatorische Trennungen – reicht eine einzelne Domain oft nicht mehr aus. Active Directory bietet dafür skalierbare Strukturen:

***

### Einzelne Domain

In einer einfachen Umgebung wird meist eine einzige Domain genutzt, z. B. `psy.local`. Diese verwaltet zentral alle Benutzer, Gruppen und Richtlinien.

<figure><img src="../../../../.gitbook/assets/grafik (6).png" alt=""><figcaption></figcaption></figure>

***

### Trees (Domänenbäume)

Ein **Tree** besteht aus mehreren **Domänen mit gemeinsamer Namenshierarchie** (Namespace), z. B.:

* `thm.local` (Root-Domain)
* `uk.thm.local` (Subdomain)
* `us.thm.local` (Subdomain)

Diese Struktur ermöglicht **logische Trennung** z. B. nach Regionen oder Abteilungen, wobei jede Domain ihre eigenen Domain Controller, Benutzer, GPOs und Admins hat.

<figure><img src="../../../../.gitbook/assets/grafik (7).png" alt=""><figcaption></figcaption></figure>

#### Vorteile:

* Bessere Verwaltung und Delegierung für verschiedene Standorte
* Eigenständige Richtlinien und Admin-Teams
* Getrennte Sicherheitsgrenzen pro Domain

#### Rollen:

| Rolle             | Gültigkeitsbereich                             |
| ----------------- | ---------------------------------------------- |
| Domain Admins     | Verwalten **eine** Domain                      |
| Enterprise Admins | Verwalten **alle Domains im Tree oder Forest** |

***

### Forests (Wälder)

Ein **Forest** ist die oberste logische Struktur in Active Directory. Er besteht aus **einem oder mehreren Trees**, auch mit **unterschiedlichen Namenräumen**:

Beispiel:

* `psy.local` Tree (mit Subdomains)
* `ki.local` Tree (eigenständige Firma)

<figure><img src="../../../../.gitbook/assets/grafik (8).png" alt=""><figcaption></figcaption></figure>

#### Vorteile:

* Unternehmen mit mehreren Marken oder Akquisitionen können getrennt bleiben
* Gemeinsame Nutzung von Ressourcen über **Trusts** möglich
* Zentrale Verwaltung über **Enterprise Admins**

***

### Trust Relationships (Vertrauensstellungen)

#### Grundprinzip:

Ein **Trust** erlaubt es einem Benutzer aus Domain A, auf Ressourcen in Domain B zuzugreifen – **sofern er autorisiert wird**.

**🡆 Einseitige Vertrauensstellung (One-Way Trust)**

<figure><img src="../../../../.gitbook/assets/grafik (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Bedeutet: _Benutzer in B können autorisiert werden, Ressourcen in A zu nutzen. Nicht umgekehrt._
{% endhint %}

**Zweiseitige Vertrauensstellung (Two-Way Trust)**

<figure><img src="../../../../.gitbook/assets/grafik (10).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Beide Domains vertrauen sich gegenseitig und können Ressourcen gemeinsam nutzen.
{% endhint %}

#### Standardverhalten:

* Domains in einem **Tree** oder **Forest** erhalten **automatisch eine zweiseitige Vertrauensstellung**.
* Externe Trusts zu anderen Forests müssen **manuell eingerichtet** werden.

{% hint style="info" %}
Ein Trust **ermöglicht** Zugriff – er **vergibt ihn aber nicht automatisch**. Ressourcenfreigabe und Berechtigungen müssen **explizit** gesetzt werden.
{% endhint %}

***

### Zusammenfassung

| Struktur   | Beschreibung                                             | Beispiel                       |
| ---------- | -------------------------------------------------------- | ------------------------------ |
| **Domain** | Grundlegende Verwaltungseinheit                          | `thm.local`                    |
| **Tree**   | Verbund von Domains mit gemeinsamem Namespace            | `uk.thm.local`, `us.thm.local` |
| **Forest** | Verbund mehrerer Trees, auch mit unterschiedlichen Namen | `thm.local` + `mht.local`      |
| **Trust**  | Beziehung zum Austausch von Berechtigungen               | Einseitig oder zweiseitig      |

***

### Visualisierung

<figure><img src="../../../../.gitbook/assets/grafik (11).png" alt=""><figcaption></figcaption></figure>

Die Visualisierung stellt ein erweitertes Active Directory-Szenario mit mehreren Domänen und einer gerichteten Vertrauensstellung dar. Auf der höchsten logischen Ebene befindet sich ein sogenannter **Forest**, der hier zwei unabhängige **Trees** (Domänenhierarchien mit eigener Namensstruktur) enthält: `psy.local` und `ki.local`.

Innerhalb des ersten Trees `psy.local` existieren zwei Subdomains: `x1.psy.local` und `x2.psy.local`. Ebenso besteht der zweite Tree `ki.local` aus den Subdomains `de.ki.local` und `fr.ki.local`. Diese Aufteilung in Trees und Subdomains bildet eine hierarchisch organisierte AD-Struktur, wie sie in größeren Unternehmen oder Konzernen üblich ist, etwa bei geographischer oder organisatorischer Trennung.

In der Visualisierung ist zusätzlich eine **gerichtete Trust-Beziehung** zwischen `x1.psy.local` und `fr.ki.local` dargestellt. Der Pfeil weist von `x1.psy.local` nach `fr.ki.local`, was bedeutet, dass **Benutzer aus `fr.ki.local` autorisierten Zugriff auf Ressourcen in `x1.psy.local` erhalten können**, nicht jedoch umgekehrt. Diese Art von Trusts nennt man **einseitig** oder **gerichtet**, da sie nur in eine Richtung funktionieren.

Solche gezielten Vertrauensstellungen ermöglichen es beispielsweise, Geschäftsbereiche mit eigener Domänenstruktur bestimmte Ressourcen oder Dienste eines anderen Bereichs zu nutzen, ohne die gesamte Verwaltung und Authentifizierung offenzulegen. Dies erlaubt eine granular kontrollierte Zusammenarbeit in komplexen AD-Umgebungen.

***

{% hint style="info" %}
### Sicherheit & Verwaltung

* Verwenden Sie Trees für **regionale oder funktionale Trennung**.
* Nutzen Sie Forests bei **Organisationsübergreifenden** Strukturen.
* **Minimieren Sie Trusts** nur auf notwendige Domains.
* **Überwachen** und **auditieren** Sie Trust-bezogene Aktivitäten regelmäßig.
{% endhint %}
