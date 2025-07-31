# detect endpoint threats via host-based monitoring and log analysis.

## core detection rules
```xml
<!-- detect suspicious powershell execution -->
<rule id="100001" level="10">
  <if_group>windows</if_group>
  <field name="win.eventdata.image">powershell.exe</field>
  <field name="win.eventdata.commandLine">-enc|-hidden|-bypass</field>
  <description>Suspicious PowerShell execution detected</description>
</rule>

<!-- detect lateral movement via psexec -->
<rule id="100002" level="12">
  <if_group>windows</if_group>
  <field name="win.eventdata.serviceName">PSEXESVC</field>
  <description>Lateral movement via PSExec detected</description>
</rule>

<!-- detect privilege escalation -->
<rule id="100003" level="10">
  <if_group>windows</if_group>
  <field name="win.eventdata.eventID">4672</field>
  <field name="win.eventdata.privilegeList">SeDebugPrivilege</field>
  <description>Debug privilege escalation detected</description>
</rule>

<!-- detect file integrity violations -->
<rule id="100004" level="8">
  <if_group>syscheck</if_group>
  <field name="syscheck.path">/etc/passwd|/etc/shadow</field>
  <description>Critical system file modified</description>
</rule>
```

## vulnerability detection
```xml
<!-- outdated software detection -->
<rule id="100010" level="7">
  <if_group>vulnerability-detector</if_group>
  <field name="vulnerability.severity">high|critical</field>
  <description>High/Critical vulnerability detected</description>
</rule>

<!-- weak SSH configuration -->
<rule id="100011" level="5">
  <if_group>sshd</if_group>
  <field name="sshd.invalid_user">.*</field>
  <description>SSH login attempt with invalid user</description>
</rule>
```

## malware detection configuration
```xml
<!-- ClamAV integration -->
<rootcheck>
  <disabled>no</disabled>
  <check_files>yes</check_files>
  <check_trojans>yes</check_trojans>
  <check_dev>yes</check_dev>
  <check_sys>yes</check_sys>
  <check_pids>yes</check_pids>
  <check_ports>yes</check_ports>
  <check_if>yes</check_if>
</rootcheck>

<!-- file integrity monitoring -->
<syscheck>
  <disabled>no</disabled>
  <frequency>43200</frequency>
  <directories check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
  <directories check_all="yes" realtime="yes">/bin,/sbin</directories>
  <ignore>/etc/mtab</ignore>
  <ignore>/etc/hosts.deny</ignore>
  <ignore>/etc/mail/statistics</ignore>
</syscheck>
```

## active response automation
```xml
<!-- automatic IP blocking -->
<active-response>
  <disabled>no</disabled>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5712,5720</rules_id>
  <timeout>600</timeout>
</active-response>

<!-- malware quarantine -->
<active-response>
  <disabled>no</disabled>
  <command>quarantine</command>
  <location>local</location>
  <rules_id>100001,100002</rules_id>
</active-response>

<!-- user account disable -->
<active-response>
  <disabled>no</disabled>
  <command>disable-account</command>
  <location>local</location>
  <rules_id>5503,5551</rules_id>
</active-response>
```

## threat hunting queries
```json
# suspicious process execution
GET wazuh-alerts*/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"data.win.eventdata.image": "cmd.exe"}},
        {"wildcard": {"data.win.eventdata.commandLine": "*net user*"}}
      ]
    }
  }
}

# lateral movement detection
GET wazuh-alerts*/_search  
{
  "query": {
    "bool": {
      "must": [
        {"match": {"data.win.eventdata.eventID": "4648"}},
        {"range": {"@timestamp": {"gte": "now-1h"}}}
      ]
    }
  },
  "aggs": {
    "by_source": {
      "terms": {"field": "agent.ip"}
    }
  }
}
```

## custom decoders
```xml
<!-- custom log format decoder -->
<decoder name="custom-app">
  <program_name>custom-app</program_name>
</decoder>

<decoder name="custom-app-login">
  <parent>custom-app</parent>
  <regex>Login attempt from (\S+) for user (\S+)</regex>
  <order>srcip,user</order>
</decoder>

<!-- JSON log decoder -->
<decoder name="json-decoder">
  <plugin_decoder>JSON_Decoder</plugin_decoder>
</decoder>
```

## performance optimization
```xml
<!-- agent configuration -->
<client>
  <notify_time>10</notify_time>
  <time-reconnect>60</time-reconnect>
  <auto_restart>yes</auto_restart>
  <queue_size>131072</queue_size>
</client>

<!-- log analysis optimization -->
<analysisd>
  <log_alert_level>3</log_alert_level>
  <email_alert_level>12</email_alert_level>
  <stats>0</stats>
  <memory_size>8192</memory_size>
</analysisd>
```

## integration with external systems
```bash
# splunk integration
/var/ossec/bin/ossec-logcollector -c /var/ossec/etc/ossec.conf | \
/opt/splunkforwarder/bin/splunk add oneshot

# elasticsearch integration
curl -X PUT "localhost:9200/wazuh-alerts-*" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "properties": {
      "timestamp": {"type": "date"},
      "rule.id": {"type": "integer"},
      "agent.ip": {"type": "ip"}
    }
  }
}'

# SOAR platform webhook
output_format = json
output_to = /var/ossec/logs/alerts/alerts.json
```

## compliance monitoring
```xml
<!-- PCI DSS compliance -->
<rule id="100020" level="10">
  <if_group>pci_dss</if_group>
  <field name="pci_dss">2.2,6.5.1</field>
  <description>PCI DSS requirement violation</description>
</rule>

<!-- GDPR compliance -->
<rule id="100021" level="12">
  <if_group>gdpr</if_group>
  <field name="gdpr">IV_32.2</field>
  <description>GDPR personal data access violation</description>
</rule>
```

[!] tune rule sensitivity to reduce false positives in production
(._.) agent overhead increases with extensive monitoring - balance coverage vs performance

