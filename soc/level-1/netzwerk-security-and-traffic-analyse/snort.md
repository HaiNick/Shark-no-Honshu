# detect network intrusions via signature-based pattern matching.

## essential snort rules
```bash
# detect nmap port scans
alert tcp any any -> $HOME_NET any (msg:"NMAP TCP scan"; flags:S,12; threshold: type both, track by_src, count 5, seconds 60; sid:1000001; rev:1;)

# detect DNS tunneling
alert udp any any -> any 53 (msg:"DNS Tunneling Detected"; content:"|01 00 00 01 00 00 00 00 00 00|"; depth:10; content:"|00 01 00 01|"; distance:0; pcre:"/[a-f0-9]{32,}/"; sid:1000002; rev:1;)

# detect powershell download cradles
alert tcp any any -> any 80 (msg:"PowerShell Download Cradle"; flow:established,to_server; content:"powershell"; content:"downloadstring"; distance:0; within:100; sid:1000003; rev:1;)

# detect lateral movement via psexec
alert tcp any any -> $HOME_NET 445 (msg:"Lateral Movement PSExec"; flow:established,to_server; content:"PSEXESVC"; sid:1000004; rev:1;)

# detect credential stuffing attacks
alert tcp any any -> any 80 (msg:"Credential Stuffing Attack"; flow:established,to_server; content:"POST"; http_method; content:"login"; http_uri; threshold:type both, track by_src, count 50, seconds 60; sid:1000005; rev:1;)
```

## malware detection rules
```bash
# detect cobalt strike beacon
alert tcp any any -> any any (msg:"Cobalt Strike Beacon"; flow:established; content:"beacon"; depth:6; sid:1000010; rev:1;)

# detect emotet C2 communication
alert tcp any any -> any any (msg:"Emotet C2 Communication"; flow:established,to_server; content:"application/x-www-form-urlencoded"; http_header; content:"Cookie"; http_header; sid:1000011; rev:1;)

# detect ransomware file extensions
alert tcp any any -> any 445 (msg:"Ransomware File Extension"; flow:established,to_server; content:".locked"; sid:1000012; rev:1;)
alert tcp any any -> any 445 (msg:"Ransomware File Extension"; flow:established,to_server; content:".encrypt"; sid:1000013; rev:1;)
```

## web application attack detection
```bash
# SQL injection attempts
alert tcp any any -> any 80 (msg:"SQL Injection Attempt"; flow:established,to_server; content:"union"; nocase; content:"select"; nocase; distance:0; within:20; sid:1000020; rev:1;)

# cross-site scripting (XSS)
alert tcp any any -> any 80 (msg:"XSS Attack Detected"; flow:established,to_server; content:"<script"; nocase; http_uri; sid:1000021; rev:1;)

# directory traversal
alert tcp any any -> any 80 (msg:"Directory Traversal"; flow:established,to_server; content:"../"; http_uri; sid:1000022; rev:1;)

# web shell upload attempts
alert tcp any any -> any 80 (msg:"Web Shell Upload"; flow:established,to_server; content:"POST"; http_method; content:".php"; http_uri; content:"system"; http_client_body; sid:1000023; rev:1;)
```

## configuration optimization
```bash
# snort.conf performance tuning
config detection: search-method ac-split search-optimize max-pattern-len 20
config event_queue: max_queue 8 log 3 order_events content_length
config pcre: match-limit 3500 match-limit-recursion 1500

# preprocessor configuration
preprocessor http_inspect_server: server default \
    profile apache \
    ports { 80 3128 8080 } \
    server_flow_depth 65535 \
    client_flow_depth 65535 \
    post_depth 65535

# reputation preprocessor
preprocessor reputation: \
    memcap 500, \
    priority whitelist, \
    nested_ip inner, \
    whitelist $WHITE_LIST_PATH/white_list.rules, \
    blacklist $BLACK_LIST_PATH/black_list.rules
```

## automated response integration
```bash
# integrate with pfSense for IP blocking
# response script: /usr/local/bin/block_ip.sh
#!/bin/bash
IP=$1
/usr/local/bin/pfctl -t snort_blocked_ips -T add $IP
logger "Snort: Blocked IP $IP via pfSense"

# configure in snort.conf
output alert_fast: /var/log/snort/alert | /usr/local/bin/block_ip.sh

# integration with SOAR platforms
output alert_json: /var/log/snort/alert.json
# SOAR reads JSON alerts for automated playbook execution
```

## rule management
```bash
# update rules automatically
cd /etc/snort/rules/
oinkmaster.pl -C /etc/oinkmaster.conf -o /etc/snort/rules/

# test rules before deployment
snort -T -c /etc/snort/snort.conf

# performance testing
snort -q -A console -c /etc/snort/snort.conf -i eth0 --daq-dir=/usr/local/lib/daq/ --daq afpacket

# rule statistics
snort --dump-dynamic-rules
```

## log analysis queries
```bash
# top alert types
cat /var/log/snort/alert | cut -d']' -f2 | cut -d'{' -f1 | sort | uniq -c | sort -nr | head -20

# most active source IPs
grep -E '\[[0-9]+:[0-9]+:[0-9]+\]' /var/log/snort/alert | \
grep -oE '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | \
sort | uniq -c | sort -nr | head -10

# alerts by time of day
cat /var/log/snort/alert | awk '{print $1, $2}' | sort | uniq -c
```

## custom rule development
```bash
# rule syntax
alert tcp $EXTERNAL_NET any -> $HOME_NET any (
    msg:"Custom Rule Description"; 
    flow:established,to_server; 
    content:"suspicious_string"; 
    nocase; 
    offset:0; 
    depth:100; 
    reference:url,threat_intel_source; 
    classtype:trojan-activity; 
    sid:9000001; 
    rev:1;
)

# testing custom rules
snort -A console -q -c /etc/snort/snort.conf -k none -r test.pcap
```

## integration with threat intelligence
```bash
# reputation lists in pulledpork
echo "url=https://rules.emergingthreats.net/|emerging.rules.tar.gz" >> /etc/pulledpork/pulledpork.conf
echo "url=https://www.snort.org/|community.rules.tar.gz" >> /etc/pulledpork/pulledpork.conf

# IP reputation integration
echo "158.69.241.129" >> /etc/snort/rules/iplists/black_list.rules
```

[!] high-traffic environments require hardware acceleration and rule tuning
(._.) signature-based detection misses zero-day attacks - combine with behavioral analysis

