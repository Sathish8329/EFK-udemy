# Elasticsearch Index Lifecycle Management (ILM) Setup

## Port Forwarding

Start port forwarding to access Elasticsearch:
```
kubectl port-forward svc/elasticsearch-master 9200:9200 -n efk
```

## Now open new terminal
# Verify connection
```
curl localhost:9200
```

```
 curl -X GET "localhost:9200/_cat/indices?v"
```

## Cleanup Old Indices
## Delete indices older than 7 days
```
curl -X DELETE "localhost:9200/cloudnloud-2025.01.1*"
curl -X DELETE "localhost:9200/cloudnloud-2025.01.2*"
```

## Create ILM Policy
## Set up automatic index management:
```
curl -X PUT "localhost:9200/_ilm/policy/logs-policy" -H 'Content-Type: application/json' -d'{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_size": "2GB",
            "max_age": "7d"
          }
        }
      },
      "delete": {
        "min_age": "7d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}'
{"acknowledged":true}{"acknowledged":true}{"acknowledged":true}
```

## This policy will:

- Create new index when size exceeds 2GB or age exceeds 7 days
- Automatically delete indices older than 7 days
- Prevent disk space issues through automated log retention
