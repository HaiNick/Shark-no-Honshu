---
description: https://www.cyberciti.biz/faq/understanding-etcshadow-file/
---

# /etc/shadow

## Aufbau

Die Informationen werden durch ":" getrennt.

<figure><img src="https://www.cyberciti.biz/faqs/uploaded_images/shadow-file-718705.png" alt=""><figcaption></figcaption></figure>

1. **Username** : A valid account name, which exist on the system.
2. **Password** : Your encrypted password is in hash format. The password should be minimum 15-20 characters long including special characters, digits, lower case alphabetic and more. Usually password format is set to $id$salt$hashed, The $id is the algorithm prefix used On GNU/Linux as follows
   1. **$1$** is MD5
   2. **$2a$** is Blowfish
   3. **$2y$** is Blowfish
   4. **$5$** is SHA-256
   5. **$6$** is SHA-512
   6. **$y$** is yescrypt
3. **Last password change (lastchanged)** : The date of the last password change, expressed as the number of days since Jan 1, 1970 (Unix time). The value 0 has a special meaning, which is that the user should change her password the next time she will log in the system. An empty field means that password aging features are disabled.
4. **Minimum** : The minimum number of days required between password changes i.e. the number of days left before the user is allowed to change her password again. An empty field and value 0 mean that there are no minimum password age.
5. **Maximum** : The maximum number of days the password is valid, after that user is forced to change her password again.
6. **Warn** : The number of days before password is to expire that user is warned that his/her password must be changed
7. **Inactive** : The number of days after password expires that account is disabled.
8. **Expire** : The date of expiration of the account, expressed as the number of days since Jan 1, 1970.

## Aufbau der Encryption

Usereintrag in /etc/passwd

<pre data-overflow="wrap"><code><strong>user123:x:1001:1002:Testuser,555,123456789,999123457:/home/sai:/bin/bash
</strong></code></pre>

Dazugehöriger Eintrag in /etc/shadow

<pre data-overflow="wrap"><code><strong>user123:$6$YTJ7JKnfsB4esnbS$5XvmYk2.GXVWhDo2TYGN2hCitD/wU9Kov.uZD8xsnleuf1r0ARX3qodIKiDsdoQA444b8IMPMOnUWDmVJVkeg1:19446:0:99999:7:::
</strong></code></pre>

* **user123** – User ID
* **$6** – The hashing algorithm prefix used for this password. In this case, it is a SHA-512 hash (512 bits). It was originally developed by Ulrich Drepper for GNU libc. Supported on Linux but not common elsewhere. Acceptable for new hashes. The default CPU time cost parameter is 5000, which is too low for modern hardware.
* **$YTJ7JKnfsB4esnbS** – The salt used to encrypt the password and it is chosen at random (6 to 96 bits).
* **$5XvmYk2.GXVWhDo2TYGN2hCitD/wU9Kov.uZD8xsnleuf1r0ARX3qodIKiDsdoQA444b8IMPMOnUWDmVJVkeg1** – The encrypted hash of the password for the user is named ‘sai’. Then, the salt and the unencrypted password are combined and encrypted to generate the encrypted hash of the password. Why use salt? It prevents two users with the same password from having duplicate entries in the /etc/shadow file. Say, if users named ‘sai’ and ‘ram’ both use ‘abracadabra’ as their passwords, their encrypted passwords in /etc/shadow will be different if their salts are different.

## A note about yescrypt on modern Linux distro such as Debian 11 and others

yescrypt is a scalable passphrase hashing scheme designed by Solar Designer, which is based on Colin Percival’s scrypt. Recommended for new hashes and now default on Debian Linux 11. Here is how typical entry look for the yescrypt:

{% code overflow="wrap" %}
```
user123:$y$j9T$5KGIS/2Ug.47GjW0jHOIB/$XwYUafYPh/petN8gKSJuLt5CEbBya3dW3pIgwrS3eJB:19447:0:99999:7:::
```
{% endcode %}

* user123 – User ID
* **$y$** – yescrypt
* $y$j9T$5KGIS/2Ug.47GjW0jHOIB/$XwYUafYPh/petN8gKSJuLt5CEbBya3dW3pIgwrS3eJB – Hashed passphrase format (\\$y\\$\[./A-Za-z0-9]+\\$\[./A-Za-z0-9]{,86}\\$\[./A-Za-z0-9]{43})
* Maximum passphrase length – unlimited
* Hash size – 256 bits
* Salt size – up to 512 bits

## Passwort manuell generieren

Passwort generieren mit

`openssl passwd -6 -salt 'salt' 'password'`

Erzeugter Hash (hier sha256) einfügen
