# Routing

#### **OSPF (Open Shortest Path First)**

**OSPF** ist ein _Link-State-Routing-Protokoll_, das innerhalb autonomer Systeme (Interior Gateway Protocol, IGP) verwendet wird. Es ermöglicht Routern, Informationen über den Zustand ihrer Verbindungen (Links) auszutauschen und daraus eine vollständige Übersicht über die Netzwerktopologie zu erstellen.

Jeder Router baut mithilfe des **Dijkstra-Algorithmus** eine eigene Routing-Tabelle auf, basierend auf dem effizientesten (kostenärmsten) Pfad zu jedem Ziel. OSPF reagiert schnell auf Änderungen im Netzwerk und ist besonders für große, komplexe Netzwerke geeignet.

**Merkmale:**

* Offener Standard (nicht proprietär)
* Hierarchische Struktur durch Areas (z. B. Backbone Area 0)
* Schnelle Konvergenz
* Unterstützt VLSM und CIDR

***

#### **EIGRP (Enhanced Interior Gateway Routing Protocol)**

**EIGRP** ist ein von Cisco entwickeltes hybrides Routing-Protokoll, das Merkmale von Distance-Vector- und Link-State-Protokollen kombiniert. Es tauscht nur bei Änderungen inkrementelle Updates aus, wodurch es sehr effizient ist.

Routing-Entscheidungen basieren auf einem **metrischen Wert**, der u. a. Bandbreite, Verzögerung, Zuverlässigkeit und MTU berücksichtigt. Dadurch kann EIGRP den besten Pfad dynamisch bestimmen.

**Merkmale:**

* Cisco-proprietär (inzwischen teilweise offen dokumentiert)
* Sehr schnelle Konvergenz
* Unterstützt uneingeschränkt VLSM und CIDR
* Verwendet den DUAL-Algorithmus (Diffusing Update Algorithm) zur Pfadberechnung

***

#### **BGP (Border Gateway Protocol)**

**BGP** ist das **zentrale Routing-Protokoll des Internets**. Es wird verwendet, um Routing-Informationen zwischen verschiedenen autonomen Systemen (AS) auszutauschen – z. B. zwischen Internetanbietern (ISPs).

BGP ist ein **Path-Vector-Protokoll** und entscheidet über Routen basierend auf einer Vielzahl von Attributen wie AS-Pfadlänge, Routing-Richtlinien und Präferenzen. Es ist für Skalierbarkeit und Kontrolle optimiert, nicht für schnelle Konvergenz.

**Merkmale:**

* Exterior Gateway Protocol (EGP)
* Skalierbar für weltweite Netzwerke
* Unterstützt Routing-Policies und Pfadmanipulation
* Komplex in Konfiguration und Wartung

***

#### **RIP (Routing Information Protocol)**

**RIP** ist ein einfaches Distance-Vector-Routing-Protokoll, das in kleinen bis mittleren Netzwerken eingesetzt wird. Es verwendet die **Anzahl der Hops** (Sprünge über Router) als Metrik – maximal 15 Hops sind erlaubt, was die Einsatzreichweite stark begrenzt.

RIP aktualisiert seine Routing-Tabellen regelmäßig (alle 30 Sekunden) durch Broadcasts an benachbarte Router, was zu langsamem Konvergenzverhalten führt.

**Merkmale:**

* Einfach zu konfigurieren
* Unterstützt RIPv1 (klassenbasiert) und RIPv2 (klassenlos, VLSM-fähig)
* Langsame Konvergenz
* Begrenzte Skalierbarkeit
