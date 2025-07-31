# detect malware via pattern matching and behavioral analysis.

## essential yara rules
```yara
# detect common malware families
rule Emotet_Malware {
    meta:
        author = "SOC Team"
        description = "Detects Emotet banking trojan"
        threat_level = "high"
    strings:
        $api1 = "VirtualAlloc" ascii
        $api2 = "CreateProcessA" ascii  
        $string1 = "Content-Type: multipart/form-data" ascii
        $string2 = { 8B 4D 08 8B 45 0C 89 01 }
    condition:
        all of ($api*) and any of ($string*)
}

rule Powershell_Obfuscation {
    meta:
        description = "Detects obfuscated PowerShell"
        severity = "medium"
    strings:
        $enc1 = "-EncodedCommand" ascii nocase
        $enc2 = "FromBase64String" ascii nocase
        $bypass = "ExecutionPolicy Bypass" ascii nocase
        $hidden = "-WindowStyle Hidden" ascii nocase
    condition:
        any of them
}

rule Cobalt_Strike_Beacon {
    meta:
        description = "Detects Cobalt Strike beacon"
        threat_level = "critical"
    strings:
        $cs1 = "beacon.dll" ascii
        $cs2 = "\x2e\x2f\x2e\x2f" 
        $cs3 = "User-Agent: Mozilla/5.0"
        $metadata = { 00 01 00 01 00 02 }
    condition:
        any of ($cs*) or $metadata
}
```

## threat hunting rules
```yara  
rule Living_Off_Land_Binaries {
    meta:
        description = "Detects LOLbin usage"
        technique = "T1218"
    strings:
        $certutil = "certutil" ascii nocase
        $bitsadmin = "bitsadmin" ascii nocase  
        $regsvr32 = "regsvr32" ascii nocase
        $mshta = "mshta" ascii nocase
        $download = "download" ascii nocase
        $url = "http" ascii nocase
    condition:
        any of ($certutil, $bitsadmin, $regsvr32, $mshta) and ($download or $url)
}

rule Persistence_Registry_Keys {
    meta:
        description = "Detects registry persistence"
        technique = "T1547.001"
    strings:
        $run1 = "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run" ascii nocase
        $run2 = "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\RunOnce" ascii nocase
        $service = "SYSTEM\\CurrentControlSet\\Services" ascii nocase
    condition:
        any of them
}
```

## automated scanning
```bash
# scan directory for malware
yara -r /path/to/rules/ /path/to/scan/

# scan with specific rule file
yara malware_rules.yar suspicious_file.exe

# scan memory dump
yara memory_rules.yar memory_dump.raw

# batch scanning with results  
find /var/log -name "*.log" -exec yara web_shell_rules.yar {} \; > scan_results.txt
```

## integration with SIEM
```python
# python script for automated yara scanning
import yara
import os
import syslog

def scan_file(filepath, rules):
    matches = rules.match(filepath)
    if matches:
        for match in matches:
            alert = f"YARA Alert: {match.rule} detected in {filepath}"
            syslog.syslog(syslog.LOG_ALERT, alert)
            return True
    return False

# compile rules once
rules = yara.compile(filepaths={
    'malware': '/opt/yara/malware.yar',
    'webshells': '/opt/yara/webshells.yar'
})

# scan incoming files
for filename in os.listdir('/incoming/'):
    scan_file(f'/incoming/{filename}', rules)
```

## rule development workflow
1. **analyze malware sample**: extract strings, API calls, behaviors
2. **identify unique patterns**: avoid false positives with common strings
3. **test rule accuracy**: validate against known good/bad samples
4. **optimize performance**: use efficient string matching techniques
5. **document metadata**: author, description, technique mapping

## performance optimization
```yara
rule Optimized_Rule {
    meta:
        description = "Example of optimized rule"
    strings:
        // use specific strings instead of wildcards
        $specific = "unique_malware_string" ascii
        // limit regex complexity  
        $regex = /malware[0-9]{2,4}/ ascii
        // use case-sensitive when possible
        $case_sensitive = "ExactString" ascii
    condition:
        // use filesize limits
        filesize < 10MB and $specific
}
```

## common rule patterns
```yara
# file hash matching
import "hash"
rule Known_Malware_Hash {
    condition:
        hash.md5(0, filesize) == "5d41402abc4b2a76b9719d911017c592"
}

# import table analysis
import "pe"
rule Suspicious_Imports {
    condition:
        pe.imports("kernel32.dll", "VirtualAlloc") and
        pe.imports("kernel32.dll", "WriteProcessMemory")
}

# entropy detection for packed files
import "math"
rule High_Entropy_File {
    condition:
        math.entropy(0, filesize) >= 7.0
}
```

[!] test rules against large file sets to avoid performance issues
(._.) false positives damage analyst confidence - validate rules thoroughly