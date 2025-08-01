# detect network threats via protocol analysis and traffic monitoring.

## core detection scripts
```zeek
# detect DNS tunneling
@load base/protocols/dns

event dns_request(c: connection, msg: dns_msg, query: string, qtype: count, qclass: count) {
    if (|query| > 50 && /[0-9a-f]{8,}/ in query) {
        NOTICE([$note=DNS::Tunneling,
                $conn=c,
                $msg=fmt("Possible DNS tunneling: %s", query)]);
    }
}

# detect port scanning
@load base/frameworks/notice

global scan_threshold = 50;
global scanners: table[addr] of set[port];

event connection_attempt(c: connection) {
    local orig = c$id$orig_h;
    if (orig !in scanners)
        scanners[orig] = set();
    
    add scanners[orig][c$id$resp_p];
    
    if (|scanners[orig]| > scan_threshold) {
        NOTICE([$note=Scan::Port_Scan,
                $src=orig,
                $msg=fmt("Port scan detected from %s", orig)]);
    }
}

# detect data exfiltration
event connection_state_remove(c: connection) {
    if (c$orig$num_bytes_ip > 10000000) {  # >10MB
        NOTICE([$note=Data::Exfiltration,
                $conn=c,
                $msg=fmt("Large data transfer: %d bytes", c$orig$num_bytes_ip)]);
    }
}
```

## log analysis queries
```bash
# identify top talkers
zeek-cut id.orig_h orig_bytes < conn.log | sort | uniq -c | sort -nr | head -20

# find long connections (potential C2)
zeek-cut id.orig_h id.resp_h duration < conn.log | awk '$3 > 3600' | sort | uniq

# detect beaconing behavior
zeek-cut id.orig_h id.resp_h id.resp_p orig_bytes resp_bytes < conn.log | \
awk '{print $1, $2, $3, $4+$5}' | sort | uniq -c | sort -nr

# identify suspicious user agents
zeek-cut id.orig_h user_agent < http.log | grep -v -E "(Chrome|Firefox|Safari)" | sort | uniq

# find lateral movement
zeek-cut id.orig_h id.resp_h id.resp_p service < conn.log | \
grep -E "(445|139|3389|22)" | sort | uniq -c | sort -nr
```

## threat hunting with zeek
```zeek
# hunt for living off the land binaries
@load policy/protocols/http

event http_request(c: connection, method: string, original_URI: string, unescaped_URI: string, version: string) {
    if (/certutil|bitsadmin|powershell|wmic/ in original_URI) {
        NOTICE([$note=HTTP::Suspicious_URI,
                $conn=c,
                $msg=fmt("Suspicious tool in URI: %s", original_URI)]);
    }
}

# detect powershell download cradles
event http_request(c: connection, method: string, original_URI: string, unescaped_URI: string, version: string) {
    if (/downloadstring|invoke-webrequest|wget|curl/ in to_lower(original_URI)) {
        NOTICE([$note=HTTP::Download_Cradle,
                $conn=c,
                $msg=fmt("Download cradle detected: %s", original_URI)]);
    }
}
```

## automated alerting
```bash
# create custom notice types in local.zeek
redef Notice::types += {
    Custom::Malware_Callback,
    Custom::Credential_Stuffing,  
    Custom::SQL_Injection
};

# monitor for specific IOCs
event http_request(c: connection, method: string, original_URI: string, unescaped_URI: string, version: string) {
    local malicious_domains = set("evil.com", "malware.net", "c2server.org");
    
    if (c$id$resp_h in malicious_domains) {
        NOTICE([$note=Custom::Malware_Callback,
                $conn=c,
                $msg=fmt("Connection to known malicious domain: %s", c$id$resp_h)]);
    }
}
```

## integration with SIEM
```bash
# forward zeek logs to splunk
# in inputs.conf
[monitor:///opt/zeek/logs/current/]
sourcetype = zeek
index = network

# elasticsearch integration
filebeat.inputs:
- type: log
  paths:
    - /opt/zeek/logs/current/*.log
  fields:
    logtype: zeek
  fields_under_root: true
```

## performance tuning
```zeek
# limit memory usage
redef table_expire_interval = 1hr;
redef table_expire_delay = 10min;

# optimize for high-traffic environments  
redef Conn::default_log_rotation_interval = 1hr;
redef HTTP::default_file_extraction_limit = 100MB;

# disable unnecessary protocols
# @load policy/protocols/ftp
# @load policy/protocols/ssh
```

## common detection patterns
```bash
# certificate anomalies
zeek-cut id.orig_h id.resp_h certificate.subject < ssl.log | \
grep -E "(localhost|test|temp)" | sort | uniq

# suspicious file downloads
zeek-cut id.orig_h uri mime_type filename < files.log | \
grep -E "\.(exe|scr|bat|ps1)$" | sort | uniq

# encrypted traffic analysis
zeek-cut id.orig_h id.resp_h id.resp_p cipher < ssl.log | \
grep -v -E "(AES|CHACHA20)" | sort | uniq
```

## incident response integration
```zeek
# export IOCs for blocking
event zeek_init() {
    local ioc_file = open("iocs_to_block.txt");
    # write detected malicious IPs to file for firewall integration
}

# pcap extraction for forensics
zeek -r suspicious_traffic.pcap extract_files.zeek
```

[!] zeek generates high-volume logs - implement log rotation and archival
(._.) encrypted traffic limits visibility - focus on metadata and certificates

