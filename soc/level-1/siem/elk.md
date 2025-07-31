# detect threats via elasticsearch log correlation and kibana visualization.

## core detection queries
```json
# failed ssh login attempts
GET /auth-*/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"syslog_program": "sshd"}},
        {"match": {"syslog_message": "Failed password"}}
      ]
    }
  },
  "aggs": {
    "by_source_ip": {
      "terms": {"field": "source_ip"}
    }
  }
}

# web shell detection
GET /webserver-*/_search
{
  "query": {
    "bool": {
      "must": [
        {"range": {"response_code": {"gte": 200, "lt": 300}}},
        {"wildcard": {"request_uri": "*cmd*"}}
      ]
    }
  }
}

# privilege escalation via sudo
GET /system-*/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"program": "sudo"}},
        {"match": {"message": "COMMAND"}}
      ]
    }
  }
}
```

## threat hunting queries
```json
# hunt for lateral movement
GET /network-*/_search
{
  "query": {
    "bool": {
      "must": [
        {"terms": {"destination_port": [445, 139, 3389]}},
        {"range": {"@timestamp": {"gte": "now-1h"}}}
      ]
    }
  },
  "aggs": {
    "lateral_movement": {
      "terms": {"field": "source_ip"},
      "aggs": {
        "unique_destinations": {
          "cardinality": {"field": "destination_ip"}
        }
      }
    }
  }
}

# detect DNS tunneling
GET /dns-*/_search
{
  "query": {
    "bool": {
      "must": [
        {"range": {"query_length": {"gte": 50}}},
        {"wildcard": {"query_name": "*.*.*.*"}}
      ]
    }
  }
}
```

## detection rules (watcher)
```json
# high volume of 4625 events (failed logons)
{
  "trigger": {"schedule": {"interval": "5m"}},
  "input": {
    "search": {
      "request": {
        "search_type": "query_then_fetch",
        "indices": ["winlogbeat-*"],
        "body": {
          "query": {
            "bool": {
              "must": [
                {"match": {"event_id": "4625"}},
                {"range": {"@timestamp": {"gte": "now-5m"}}}
              ]
            }
          }
        }
      }
    }
  },
  "condition": {"compare": {"ctx.payload.hits.total": {"gt": 50}}},
  "actions": {
    "send_email": {
      "email": {
        "to": "soc@company.com",
        "subject": "High volume failed logons detected"
      }
    }
  }
}
```

## kibana dashboard queries
```kql
# security dashboard searches
failed_logons: event_id:4625 AND @timestamp:[now-1h TO now]
malware_detected: process.name:(cmd.exe OR powershell.exe) AND process.command_line:*download*
network_scans: destination_port:(21 OR 22 OR 23 OR 80 OR 443) AND source_ip:* 
suspicious_processes: process.parent.name:explorer.exe AND process.name:(*.tmp OR *.dat)
```

## logstash parsing rules
```ruby
# parse windows security logs
filter {
  if [winlogbeat][event_id] == 4624 {
    mutate {
      add_tag => ["successful_logon"]
      add_field => {"logon_type" => "%{[winlogbeat][event_data][LogonType]}"}
    }
  }
}

# enrich with threat intelligence
filter {
  if [source_ip] {
    translate {
      field => "source_ip"
      destination => "threat_intel"
      dictionary_path => "/etc/logstash/threat_intel.yaml"
    }
  }
}
```

[!] index patterns must match log source naming conventions
(._.) large result sets can impact elasticsearch cluster performance