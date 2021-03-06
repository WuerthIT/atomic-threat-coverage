| Title                | Windows Network Enumeration                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Identifies attempts to enumerate hosts in a network using the built-in Windows net.exe tool.                                                                                                                                           |
| ATT&amp;CK Tactic    |  <ul><li>[TA0007: Discovery](https://attack.mitre.org/tactics/TA0007)</li></ul>  |
| ATT&amp;CK Technique | <ul><li>[T1018: Remote System Discovery](https://attack.mitre.org/techniques/T1018)</li></ul>  |
| Data Needed          | <ul><li>[DN_0002_4688_windows_process_creation_with_commandline](../Data_Needed/DN_0002_4688_windows_process_creation_with_commandline.md)</li><li>[DN_0003_1_windows_sysmon_process_creation](../Data_Needed/DN_0003_1_windows_sysmon_process_creation.md)</li></ul>  |
| Trigger              | <ul><li>[T1018: Remote System Discovery](../Triggers/T1018.md)</li></ul>  |
| Severity Level       | low |
| False Positives      | <ul><li>Legitimate use of net.exe utility by legitimate user</li></ul>  |
| Development Status   | stable |
| References           | <ul><li>[https://eqllib.readthedocs.io/en/latest/analytics/b8a94d2f-dc75-4630-9d73-1edc6bd26fff.html](https://eqllib.readthedocs.io/en/latest/analytics/b8a94d2f-dc75-4630-9d73-1edc6bd26fff.html)</li><li>[https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1018/T1018.yaml](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1018/T1018.yaml)</li></ul>  |
| Author               | Endgame, JHasenbusch (ported for oscd.community) |


## Detection Rules

### Sigma rule

```
title: Windows Network Enumeration
id: 62510e69-616b-4078-b371-847da438cc03
status: stable
description: Identifies attempts to enumerate hosts in a network using the built-in Windows net.exe tool.
references:
    - https://eqllib.readthedocs.io/en/latest/analytics/b8a94d2f-dc75-4630-9d73-1edc6bd26fff.html
    - https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1018/T1018.yaml
author: Endgame, JHasenbusch (ported for oscd.community)
date: 2018/10/30
modified: 2019/11/11
tags:
    - attack.discovery
    - attack.t1018
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        Image|endswith: 
            - '\net.exe'
            - '\net1.exe'
        CommandLine|contains: 'view'
    filter:
        CommandLine|contains: '\\'
    condition: selection and not filter
falsepositives:
    - Legitimate use of net.exe utility by legitimate user
level: low 

```





### es-qs
    
```
((Image.keyword:(*\\\\net.exe OR *\\\\net1.exe) AND CommandLine.keyword:*view*) AND (NOT (CommandLine.keyword:*\\\\*)))
```


### xpack-watcher
    
```
curl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_watcher/watch/Windows-Network-Enumeration <<EOF\n{\n  "metadata": {\n    "title": "Windows Network Enumeration",\n    "description": "Identifies attempts to enumerate hosts in a network using the built-in Windows net.exe tool.",\n    "tags": [\n      "attack.discovery",\n      "attack.t1018"\n    ],\n    "query": "((Image.keyword:(*\\\\\\\\net.exe OR *\\\\\\\\net1.exe) AND CommandLine.keyword:*view*) AND (NOT (CommandLine.keyword:*\\\\\\\\*)))"\n  },\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "bool": {\n              "must": [\n                {\n                  "query_string": {\n                    "query": "((Image.keyword:(*\\\\\\\\net.exe OR *\\\\\\\\net1.exe) AND CommandLine.keyword:*view*) AND (NOT (CommandLine.keyword:*\\\\\\\\*)))",\n                    "analyze_wildcard": true\n                  }\n                }\n              ],\n              "filter": {\n                "range": {\n                  "timestamp": {\n                    "gte": "now-30m/m"\n                  }\n                }\n              }\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": "root@localhost",\n        "subject": "Sigma Rule \'Windows Network Enumeration\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\n
```


### graylog
    
```
((Image.keyword:(*\\\\net.exe *\\\\net1.exe) AND CommandLine.keyword:*view*) AND (NOT (CommandLine.keyword:*\\\\*)))
```


### splunk
    
```
(((Image="*\\\\net.exe" OR Image="*\\\\net1.exe") CommandLine="*view*") NOT (CommandLine="*\\\\*"))
```


### logpoint
    
```
(event_id="1" (Image IN ["*\\\\net.exe", "*\\\\net1.exe"] CommandLine="*view*")  -(CommandLine="*\\\\*"))
```


### grep
    
```
grep -P '^(?:.*(?=.*(?:.*(?=.*(?:.*.*\\net\\.exe|.*.*\\net1\\.exe))(?=.*.*view.*)))(?=.*(?!.*(?:.*(?=.*.*\\\\.*)))))'
```



