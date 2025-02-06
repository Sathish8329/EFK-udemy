## To Create an alert in elasticsearch for eg if 500 status code need to trigger

```
# Create alert in Elasticsearch
curl -X PUT "localhost:9200/_watcher/watch/http-500-errors" -H 'Content-Type: application/json' -d'{
 "trigger": {
   "schedule": {
     "interval": "5m"
   }
 },
 "input": {
   "search": {
     "request": {
       "indices": ["cloudnloud-*"],
       "body": {
         "query": {
           "bool": {
             "must": [
               { "match": { "status": "500" }}
             ]
           }
         },
         "size": 0,
         "aggs": {
           "error_count": { "value_count": { "field": "status" }}
         }
       }
     }
   }
 },
 "condition": {
   "compare": {
     "ctx.payload.aggregations.error_count.value": {
       "gt": 10
     }
   }
 },
 "actions": {
   "email": {
     "email": {
       "to": "your-email@domain.com",
       "subject": "High number of 500 errors detected",
       "body": "More than 10 HTTP 500 errors detected in the last 5 minutes"
     }
   }
 }
}'

```

## replace the mailid 
