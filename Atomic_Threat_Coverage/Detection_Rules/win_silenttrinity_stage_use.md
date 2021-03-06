| Title                | SILENTTRINITY stager execution                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Detects SILENTTRINITY stager use                                                                                                                                           |
| ATT&amp;CK Tactic    |  <ul><li>[TA0002: Execution](https://attack.mitre.org/tactics/TA0002)</li></ul>  |
| ATT&amp;CK Technique |  This Detection Rule wasn't mapped to ATT&amp;CK Technique yet  |
| Data Needed          | <ul><li>[DN_0011_7_windows_sysmon_image_loaded](../Data_Needed/DN_0011_7_windows_sysmon_image_loaded.md)</li><li>[DN_0002_4688_windows_process_creation_with_commandline](../Data_Needed/DN_0002_4688_windows_process_creation_with_commandline.md)</li><li>[DN_0001_4688_windows_process_creation](../Data_Needed/DN_0001_4688_windows_process_creation.md)</li><li>[DN_0003_1_windows_sysmon_process_creation](../Data_Needed/DN_0003_1_windows_sysmon_process_creation.md)</li></ul>  |
| Trigger              |  There is no documented Trigger for this Detection Rule yet  |
| Severity Level       | high |
| False Positives      | <ul><li>unknown</li></ul>  |
| Development Status   | experimental |
| References           | <ul><li>[https://github.com/byt3bl33d3r/SILENTTRINITY](https://github.com/byt3bl33d3r/SILENTTRINITY)</li></ul>  |
| Author               | Aleksey Potapov, oscd.community |


## Detection Rules

### Sigma rule

```
action: global
title: SILENTTRINITY stager execution
id: 03552375-cc2c-4883-bbe4-7958d5a980be
status: experimental
description: Detects SILENTTRINITY stager use
references:
    - https://github.com/byt3bl33d3r/SILENTTRINITY
author: Aleksey Potapov, oscd.community
date: 2019/10/22
modified: 2019/11/04
tags:
    - attack.execution
detection:
    selection:
        Description|contains: 'st2stager'
    condition: selection
falsepositives:
    - unknown
level: high
---
logsource:
    category: process_creation
    product: windows
---
logsource:
    product: windows
    service: sysmon
detection:
    selection:
        EventID: 7

```





### es-qs
    
```
Description.keyword:*st2stager*\n(EventID:"7" AND Description.keyword:*st2stager*)
```


### xpack-watcher
    
```
curl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_watcher/watch/SILENTTRINITY-stager-execution <<EOF\n{\n  "metadata": {\n    "title": "SILENTTRINITY stager execution",\n    "description": "Detects SILENTTRINITY stager use",\n    "tags": [\n      "attack.execution"\n    ],\n    "query": "Description.keyword:*st2stager*"\n  },\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "bool": {\n              "must": [\n                {\n                  "query_string": {\n                    "query": "Description.keyword:*st2stager*",\n                    "analyze_wildcard": true\n                  }\n                }\n              ],\n              "filter": {\n                "range": {\n                  "timestamp": {\n                    "gte": "now-30m/m"\n                  }\n                }\n              }\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": "root@localhost",\n        "subject": "Sigma Rule \'SILENTTRINITY stager execution\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\ncurl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_watcher/watch/SILENTTRINITY-stager-execution-2 <<EOF\n{\n  "metadata": {\n    "title": "SILENTTRINITY stager execution",\n    "description": "Detects SILENTTRINITY stager use",\n    "tags": [\n      "attack.execution"\n    ],\n    "query": "(EventID:\\"7\\" AND Description.keyword:*st2stager*)"\n  },\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "bool": {\n              "must": [\n                {\n                  "query_string": {\n                    "query": "(EventID:\\"7\\" AND Description.keyword:*st2stager*)",\n                    "analyze_wildcard": true\n                  }\n                }\n              ],\n              "filter": {\n                "range": {\n                  "timestamp": {\n                    "gte": "now-30m/m"\n                  }\n                }\n              }\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": "root@localhost",\n        "subject": "Sigma Rule \'SILENTTRINITY stager execution\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\n
```


### graylog
    
```
Description.keyword:*st2stager*\n(EventID:"7" AND Description.keyword:*st2stager*)
```


### splunk
    
```
Description="*st2stager*"\n(EventID="7" Description="*st2stager*")
```


### logpoint
    
```
(event_id="1" Description="*st2stager*")\n(event_id="7" Description="*st2stager*")
```


### grep
    
```
grep -P '^.*st2stager.*'\ngrep -P '^(?:.*(?=.*7)(?=.*.*st2stager.*))'
```



