---
description: web app pentesting framework that actually works
cover: >-
  https://uploads-ssl.webflow.com/62efedb360a7998b0e43cb84/6321a0f076706854ff591093_All%20about%20BurpSuite.jpg
coverY: 0
---

# burp suite — intercept everything

java-based framework for web/mobile app penetration testing. the industry standard.

## editions

* **community** (free) — rate limited, no scanner
* **professional** ($$$) — automated vuln scanner, collaborator, unlimited  
* **enterprise** ($$$$) — continuous scanning for enterprises

## core modules

### proxy
intercept and modify HTTP/HTTPS requests between browser and target. the bread and butter.

### repeater
capture a request, modify it, send it again. perfect for testing SQL injection payloads or API fuzzing.

tryhackme room: https://tryhackme.com/room/burpsuiterepeater

### intruder  
spray endpoints with requests. brute force, fuzzing, wordlist attacks. community edition is throttled.

tryhackme room: https://tryhackme.com/room/burpsuiteintruder

### decoder
transform data — base64, URL encoding, hex, etc. handy for payload preparation.

### comparer
diff two requests/responses at word or byte level. spot differences quickly.

### sequencer
analyze randomness of tokens, session cookies, CSRF tokens. weak entropy = session hijacking opportunities.

## shortcuts

| shortcut | function |
|----------|----------|
| `CTRL + Shift + D` | dashboard |
| `CTRL + Shift + T` | target tab |
| `CTRL + Shift + P` | proxy tab |
| `CTRL + Shift + I` | intruder tab |
| `CTRL + Shift + R` | repeater tab |

## extensions

burp extender loads plugins written in java, python (jython), ruby (jruby).

bapp store: https://portswigger.net/bappstore

writing extensions: https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension

## Shortcuts

<table><thead><tr><th width="310">Shortcut</th><th>Tab</th></tr></thead><tbody><tr><td><code>CTRL + Shift + D</code></td><td>Dashboard</td></tr><tr><td><code>CTRL + Shift + T</code></td><td>Target Tab</td></tr><tr><td><code>CTRL + Shift + P</code></td><td>Proxy Tab</td></tr><tr><td><code>CTRL + Shift + I</code></td><td>Intruder Tab</td></tr><tr><td><code>CTRL + Shift + R</code></td><td>Repeater Tab</td></tr></tbody></table>

## Erweiterungen entwickeln

[https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension](https://portswigger.net/burp/extender/writing-your-first-burp-suite-extension)

## Tipps

* NTLM mit Burp : [https://san3ncrypt3d.com/2021/10/26/burp/](https://san3ncrypt3d.com/2021/10/26/burp/) + Name/Passwort unter Network>Connections>"Platform authentication"

