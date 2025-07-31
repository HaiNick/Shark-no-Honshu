# SQL

## Verbindung zu SQL-Server

{% code overflow="wrap" fullWidth="false" %}
```sql
#login auf db als user
mysql -u USERNAME -pPASSWORD -h HOSTNAMEORIP DATABASENAME 
# als root
sudo mysql

#Verfügbare Datenbanken anzeigen
show databases;

#Andere Datenbank laden
use database_name;

#Verfügbare Tabellen anzeigen
show tables;

#User anlegen und adminrechte zuweisen
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'new_user'@'localhost' WITH GRANT OPTION;

#Privilegien neu laden
FLUSH PRIVILEGES;

#Rechte des user prüfen
SELECT user, host FROM mysql.user WHERE user = 'new_user';
SHOW GRANTS FOR 'new_user'@'localhost';

#Alle User anzeigen
SELECT user, host FROM mysql.user;

#Kodierung von charactersets anzeigen
SHOW VARIABLES WHERE Variable_name LIKE 'character_set%' OR Variable_name LIKE 'collation%';



```
{% endcode %}

## SQL Joins

<figure><img src="../.gitbook/assets/Bildschirmfoto vom 2024-01-15 21-43-29.png" alt=""><figcaption></figcaption></figure>

* (INNER) JOIN

Gibt Datensätze zurück, deren Werte in beiden Tabellen übereinstimmen.

{% code title="INNER JOIN Beispiel" fullWidth="false" %}
```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
    FROM Orders
        INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```
{% endcode %}

* LEFT (OUTER) JOIN

Gibt alle Datensätze aus der linken Tabelle und die übereinstimmenden Datensätze aus der rechten Tabelle zurück

{% code title="LEFT JOIN Beispiel" %}
```
SELECT column_name(s)
    FROM table1
        LEFT JOIN table2
            ON table1.column_name = table2.column_name;
```
{% endcode %}

* RIGHT (OUTER) JOIN

Gibt alle Datensätze aus der rechten Tabelle und die übereinstimmenden Datensätze aus der linken Tabelle zurück

{% code title="RIGHT JOIN Beispiel" %}
```
SELECT column_name(s)
    FROM table1
        RIGHT JOIN table2
            ON table1.column_name = table2.column_name;
# Wird in manchen Datenbanken als RIGHT OUTER JOIN bezeichnet !
```
{% endcode %}

* FULL (OUTER) JOIN

Gibt alle Datensätze zurück, wenn es entweder in der linken oder der rechten Tabelle eine Übereinstimmung gibt.

<pre data-title="FULL OUTER JOIN Beispiel"><code>SELECT column_name(s)
<strong>    FROM table1
</strong>        FULL OUTER JOIN table2
            ON table1.column_name = table2.column_name
                WHERE condition;
# Kann sehr große Datenmengen zurückgeben !
</code></pre>

## SUBSTR, CAST, LIMIT

```
Substring ist Bedingung für Y = X wenn String:X = Auszug aus "string"

substr("string", START, LÄNGE) = X

# start = 1, länge = 1, -> gültig
für substr("Text123", 1,1) = T

# start = 2, länge = 1, -> ungültig
für substr("Text123", 2,1) = x

# start = 2, länge = 3, -> gültig
für substr("Text123", 2,3) = ext
```

```
CAST(X'ascii_code' as Text)
SUBSTR((SELECT password FROM users LIMIT 0,1),1,1) = CAST(X'54' as Text) # T
```

```
LIMIT <OFFSET> <LIMIT>
select * from x limit 0,1
```
