# User

### Verwaltung und Bereinigung von OUs und Benutzern

#### Entfernen veralteter Organisationseinheiten (OUs)

Im Laufe der Zeit kann es vorkommen, dass bestimmte Abteilungen aufgelöst oder umstrukturiert werden. Solche Änderungen sollten sich auch in der Active Directory-Struktur widerspiegeln. In unserem Beispiel existiert eine OU, die nicht mehr im Organisationsdiagramm erscheint. Diese Abteilung wurde laut Vorgabe aufgrund von Budgetkürzungen geschlossen und soll daher aus der AD gelöscht werden.

**Standardmäßig ist das Löschen von OUs durch einen Schutzmechanismus blockiert.** Wenn Sie versuchen, eine OU zu löschen, erhalten Sie eine Fehlermeldung, weil die OU gegen versehentliches Löschen geschützt ist.

**Schritte zum Löschen einer OU:**

1. **Erweiterte Funktionen aktivieren:**
   * Öffnen Sie die Konsole **Active Directory-Benutzer und -Computer**.
   * Klicken Sie auf das Menü **Ansicht** und aktivieren Sie **Erweiterte Features**.
2. **Löschschutz aufheben:**
   * Navigieren Sie zur betreffenden OU.
   * Rechtsklick → **Eigenschaften**.
   * Wechseln Sie zum Reiter **Objekt**.
   * **Deaktivieren** Sie die Option **Objekt vor zufälligem Löschen schützen**.
   * Bestätigen Sie mit OK.
3. **OU löschen:**
   * Erneut Rechtsklick auf die OU → **Löschen**.
   * Bestätigen Sie die Sicherheitsabfrage.

{% hint style="warning" %}
**Achtung:** Beim Löschen einer OU werden alle darin enthaltenen Objekte (Benutzer, Gruppen, Unter-OUs) ebenfalls gelöscht.
{% endhint %}

***

#### Synchronisierung mit der Organisationsstruktur

Nachdem veraltete OUs entfernt wurden, sollte geprüft werden, ob die Benutzerkonten den aktuellen Vorgaben entsprechen. In vielen Fällen sind:

* Benutzerkonten für nicht mehr existierende Mitarbeitende vorhanden.
* Neue Mitarbeitende noch nicht angelegt.

**Maßnahmen:**

* Nicht mehr benötigte Benutzerkonten **löschen**.
* Fehlende Benutzer **neu anlegen**.
* Optional: Passwörter zurücksetzen oder initial konfigurieren.

Diese Maßnahmen sollten regelmäßig im Rahmen der AD-Wartung erfolgen, insbesondere bei organisatorischen Veränderungen.

***

### Delegation in Active Directory

**Delegation** ermöglicht es, bestimmten Benutzern administrative Rechte über spezifische Bereiche in AD zu gewähren – ohne ihnen vollständige Domänenadministratorrechte zu erteilen. Dies ist essenziell für das Prinzip der minimalen Rechtevergabe.

#### Typisches Anwendungsbeispiel: Passwortzurücksetzung durch Helpdesk

Laut Organisationsstruktur ist **Phillip** für den IT-Support zuständig. Er soll das Recht erhalten, Passwörter in den OUs **Sales**, **Marketing** und **Management** zurückzusetzen. Dies steigert die Effizienz und entlastet zentrale Administratoren.

**Schritt-für-Schritt: Delegation einrichten**

1. **OU auswählen:** Rechtsklick auf z. B. **Sales** → **Steuerelement delegieren…**
2. **Assistent starten:**
   * Benutzer hinzufügen (z. B. **phillip** → mit **"Namen überprüfen"** bestätigen).
   * Berechtigung auswählen:\
     &#xNAN;**„Benutzern das Zurücksetzen von Kennwörtern und das Erzwingen einer Kennwortänderung bei der nächsten Anmeldung gestatten“**
   * Assistent abschließen.

Wiederholen Sie den Vorgang bei Bedarf für die anderen OUs (Marketing, Management).

***

#### Delegation testen: Passwort zurücksetzen mit PowerShell

**Passwort für Benutzer „n00b“ zurücksetzen**

```powershell
Set-ADAccountPassword n00b -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

**Passwortwechsel bei nächster Anmeldung erzwingen**

```powershell
Set-ADUser -ChangePasswordAtLogon $true -Identity n00b -Verbose
```

{% hint style="info" %}
Dieses Vorgehen stellt sicher, dass das neu gesetzte Passwort nicht dauerhaft bekannt bleibt und **n00b** bei der nächsten Anmeldung zur Änderung gezwungen wird.
{% endhint %}
