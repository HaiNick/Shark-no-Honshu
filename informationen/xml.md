# XML

[https://pypi.org/project/defusedxml/](https://pypi.org/project/defusedxml/)

Die Folgen eines Angriffs auf eine anfällige XML-Bibliothek können ziemlich dramatisch sein. Mit nur wenigen hundert Bytes XML-Daten kann ein Angreifer innerhalb von Sekunden mehrere Gigabyte Speicher belegen. Ein Angreifer kann CPUs auch mit einer kleinen bis mittelgroßen Anfrage längere Zeit beschäftigen. Unter Umständen ist es sogar möglich, auf lokale Dateien auf Ihrem Server zuzugreifen, eine Firewall zu umgehen oder Dienste zu missbrauchen, um Angriffe auf Dritte abzuwehren.

Die Angriffe nutzen und missbrauchen weniger verbreitete Funktionen von XML und seinen Parsern. Die Mehrheit der Entwickler ist mit Funktionen wie Verarbeitungsanweisungen und Entitätserweiterungen, die XML von SGML geerbt hat, nicht vertraut. Ihr Wissen über `<!DOCTYPE>` stammt meistens maximal durch Vorerfahrungen mit HTML, sie wissen aber nicht, dass eine Dokumenttypdefinition (DTD) eine HTTP-Anfrage generieren oder eine Datei aus dem Dateisystem laden kann.

## Angriffsvektoren

* billion  laughs/ exponential entity expansion

```
<!DOCTYPE xmlbomb [
<!ENTITY a "1234567890" >
<!ENTITY b "&a;&a;&a;&a;&a;&a;&a;&a;">
<!ENTITY c "&b;&b;&b;&b;&b;&b;&b;&b;">
<!ENTITY d "&c;&c;&c;&c;&c;&c;&c;&c;">
]>
<bomb>&d;</bomb>
```

* quadratic blowup entity expansion

```
<!DOCTYPE bomb [
<!ENTITY a "xxxxxxx... a couple of ten thousand chars">
]>
<bomb>&a;&a;&a;... repeat</bomb>
```

* external entity expansion (remote)

```
<!DOCTYPE external [
<!ENTITY ee SYSTEM "http://www.python.org/some.xml">
]>
<root>&ee;</root>
```

* external entity expansion (lokal)

```
<!DOCTYPE external [
<!ENTITY ee SYSTEM "file:///PATH/TO/simple.xml">
]>
<root>&ee;</root>
```

* DTD retrieval

```
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html>
    <head/>
    <body>text</body>
</html>
```







