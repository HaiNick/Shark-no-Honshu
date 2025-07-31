# OGNL (Object-Graph Navigation Language)

### **Was ist OGNL?**

Object-Graph Navigation Language (OGNL) ist eine Ausdruckssprache, die in Java-basierten Webanwendungen verwendet wird. Sie ermöglicht es Entwicklern, komplexe Operationen auf Java-Objekten durchzuführen, Werte zu extrahieren und Methoden aufzurufen. OGNL wird häufig in Frameworks wie Apache Struts 2 verwendet, um Datenbindung und andere dynamische Funktionen zu unterstützen.

### **Hauptfunktionen**

1. **Datenbindung**: OGNL kann verwendet werden, um Daten zwischen HTML-Formularen und Java-Objekten zu binden.
2. **Methodenaufrufe**: Es ermöglicht das Aufrufen von Methoden auf Java-Objekten.
3. **Eigenschaftenzugriff**: OGNL kann verwendet werden, um auf Eigenschaften von Java-Objekten zuzugreifen und diese zu modifizieren.
4. **Ausdrucksbewertung**: OGNL kann komplexe Ausdrücke evaluieren, um Werte zu berechnen und Entscheidungen zu treffen.

### Syntaxbeispiele

#### Eigenschaftenzugriff und -zuweisung

```
// Zugriff auf eine Eigenschaft
person.name

// Zuweisung einer Eigenschaft
person.name = "John"
```



Methodenaufrufe

```
// Aufrufen einer Methode ohne Parameter
person.getName()

// Aufrufen einer Methode mit Parametern
person.setName("John")
```



Arbeiten mit Listen und Arrays

```
// Zugriff auf ein Listenelement
personList[0]

// Hinzufügen eines Elements zu einer Liste
personList.add(newPerson)

// Iterieren über eine Liste
#foreach{person : personList}
  person.name
#end
```



Bedingte Ausdrücke

{% code overflow="wrap" %}
```
// Einfacher bedingter Ausdruck
person.age > 18 ? "Erwachsener" : "Minderjähriger"

// Verschachtelter bedingter Ausdruck
person.age > 18 ? (person.gender == 'M' ? "Männlicher Erwachsener" : "Weiblicher Erwachsener") : "Minderjähriger"
```
{% endcode %}



Arbeiten mit Maps

```
// Zugriff auf einen Map-Wert
personMap["name"]

// Zuweisung eines Map-Werts
personMap["age"] = 30
```

