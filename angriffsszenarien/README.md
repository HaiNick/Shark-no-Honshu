# attack scenarios — get root, document how

working exploitation chains for active engagement.

## file upload attacks
- `upload-vulnerability/` - bypass upload filters, deliver shells
- get code execution through file upload endpoints
- php, asp, jsp shells via extension/mime/content bypasses

## web application attacks  
- `webservices/sqli-sql-injection.md` - extract data, bypass auth
- `webservices/command-injection.md` - execute system commands
- `webservices/user-eingaben/xss-cross-site-scripting.md` - steal sessions, keylog
- `webservices/user-eingaben/ssrf-server-side-request-forgery.md` - pivot internal networks
- `webservices/user-eingaben/file-inclusion/` - read files, execute code

## format
each scenario contains:
- purpose and outcome
- working payloads 
- bypass techniques
- operational notes
- [!] danger warnings

speak like you just owned the box.
