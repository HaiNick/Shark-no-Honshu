# ELK Search API

### Grundlegende Suche

#### Einfacher Suchbefehl

```
GET /index_name/_search
{
  "query": {
    "match_all": {}
  }
}
```

#### Volltextsuche mit match

```
GET /index_name/_search
{
  "query": {
    "match": {
      "field_name": "search text"
    }
  }
}
```

### Bool Queries

#### Kombinierte Abfrage (must, should, must\_not)

```
GET /index_name/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "field_name1": "value1" }},
        { "match": { "field_name2": "value2" }}
      ],
      "should": [
        { "match": { "field_name3": "value3" }}
      ],
      "must_not": [
        { "match": { "field_name4": "value4" }}
      ]
    }
  }
}
```

### Filtered Queries

#### Filter in Kombination mit Bool Queries

```
GET /index_name/_search
{
  "query": {
    "bool": {
      "must": { "match": { "field_name": "value" }},
      "filter": {
        "term": { "status": "active" }
      }
    }
  }
}
```

### Aggregationen (Verbindung von Daten)

#### Einfache Aggregation

```
GET /index_name/_search
{
  "size": 0,
  "aggs": {
    "name_of_aggregation": {
      "terms": {
        "field": "field_name.keyword"
      }
    }
  }
}
```

#### Verschachtelte Aggregation

```
GET /index_name/_search
{
  "size": 0,
  "aggs": {
    "first_agg": {
      "terms": {
        "field": "field_name1.keyword"
      },
      "aggs": {
        "second_agg": {
          "avg": {
            "field": "field_name2"
          }
        }
      }
    }
  }
}
```

### Pagination

#### Einstellung der Größe und des Offsets

```
GET /index_name/_search
{
  "from": 10,
  "size": 20,
  "query": {
    "match_all": {}
  }
}
```

### Sortierung

#### Sortieren nach einem Feld

```
GET /index_name/_search
{
  "sort": [
    { "field_name": { "order": "asc" }}
  ],
  "query": {
    "match_all": {}
  }
}
```

#### Sortieren nach mehreren Feldern

```
GET /index_name/_search
{
  "sort": [
    { "field_name1": { "order": "asc" }},
    { "field_name2": { "order": "desc" }}
  ],
  "query": {
    "match_all": {}
  }
}
```

### Highlighting

#### Hervorhebung von Suchbegriffen

```
GET /index_name/_search
{
  "query": {
    "match": {
      "field_name": "search text"
    }
  },
  "highlight": {
    "fields": {
      "field_name": {}
    }
  }
}
```

### Mappings und Analysatoren

#### Mapping anzeigen

```
GET /index_name/_mapping
```

#### Analyse einer Zeichenkette

```
GET /index_name/_analyze
{
  "analyzer": "standard",
  "text": "this is a test"
}
```

### Multi-Index-Suche

#### Suche über mehrere Indizes

```
GET /index1,index2/_search
{
  "query": {
    "match_all": {}
  }
}
```

### Index Alias

Alias für Index erstellen

```
POST /_aliases
{
  "actions": [
    { "add": { "index": "index_name", "alias": "alias_name" }}
  ]
}
```

### Erweiterte Queries

#### Bereichsabfrage (Range Query)

```
GET /index_name/_search
{
  "query": {
    "range": {
      "date_field": {
        "gte": "2022-01-01",
        "lte": "2022-12-31"
      }
    }
  }
}
```

#### Wildcard Query (Platzhalterabfrage)

```
GET /index_name/_search
{
  "query": {
    "wildcard": {
      "field_name": "search*"
    }
  }
}
```

#### Fuzzy Query (Ungefähre Übereinstimmung)

```
GET /index_name/_search
{
  "query": {
    "fuzzy": {
      "field_name": {
        "value": "searh",
        "fuzziness": "AUTO"
      }
    }
  }
}
```

### Scripted Fields

#### Felder mit Skripten erstellen

```
GET /index_name/_search
{
  "query": {
    "match_all": {}
  },
  "script_fields": {
    "new_field": {
      "script": {
        "source": "doc['existing_field'].value * 2"
      }
    }
  }
}
```

### Existenzprüfung

#### Überprüfen, ob ein Dokument existiert

```
GET /index_name/_search
{
  "query": {
    "exists": {
      "field": "field_name"
    }
  }
}
```

### Weitere nützliche Funktionen

#### Scroll API (für große Datenmengen)

```
POST /index_name/_search?scroll=1m
{
  "size": 100,
  "query": {
    "match_all": {}
  }
}
```

#### Search Template (Vorlagen verwenden)

```
POST /_search/template
{
  "id": "template_id",
  "params": {
    "field": "value"
  }
}
```

### Indizes- und Dokumentenmanagement

#### Index erstellen

```
PUT /index_name
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "field_name": {
        "type": "text"
      }
    }
  }
}
```

#### Dokument hinzufügen

```
POST /index_name/_doc/1
{
  "field_name": "value"
}
```

#### Dokument aktualisieren

```
POST /index_name/_update/1
{
  "doc": {
    "field_name": "new_value"
  }
}
```

#### Dokument löschen

```
DELETE /index_name/_doc/1
```

#### Index löschen

```
DELETE /index_name
```



