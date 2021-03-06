| Title                | Suspicious Debugger Registration Cmdline                                                                                                                                                 |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | Detects the registration of a debugger for a program that is available in the logon screen (sticky key backdoor).                                                                                                                                           |
| ATT&amp;CK Tactic    |  <ul><li>[TA0003: Persistence](https://attack.mitre.org/tactics/TA0003)</li><li>[TA0004: Privilege Escalation](https://attack.mitre.org/tactics/TA0004)</li></ul>  |
| ATT&amp;CK Technique | <ul><li>[T1015: Accessibility Features](https://attack.mitre.org/techniques/T1015)</li></ul>  |
| Data Needed          | <ul><li>[DN_0002_4688_windows_process_creation_with_commandline](../Data_Needed/DN_0002_4688_windows_process_creation_with_commandline.md)</li><li>[DN_0003_1_windows_sysmon_process_creation](../Data_Needed/DN_0003_1_windows_sysmon_process_creation.md)</li></ul>  |
| Trigger              | <ul><li>[T1015: Accessibility Features](../Triggers/T1015.md)</li></ul>  |
| Severity Level       | high |
| False Positives      | <ul><li>Penetration Tests</li></ul>  |
| Development Status   | experimental |
| References           | <ul><li>[https://blogs.technet.microsoft.com/jonathantrull/2016/10/03/detecting-sticky-key-backdoors/](https://blogs.technet.microsoft.com/jonathantrull/2016/10/03/detecting-sticky-key-backdoors/)</li></ul>  |
| Author               | Florian Roth |


## Detection Rules

### Sigma rule

```
title: Suspicious Debugger Registration Cmdline
id: ae215552-081e-44c7-805f-be16f975c8a2
status: experimental
description: Detects the registration of a debugger for a program that is available in the logon screen (sticky key backdoor).
references:
    - https://blogs.technet.microsoft.com/jonathantrull/2016/10/03/detecting-sticky-key-backdoors/
tags:
    - attack.persistence
    - attack.privilege_escalation
    - attack.t1015
author: Florian Roth
date: 2019/09/06
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        CommandLine:
            - '*\CurrentVersion\Image File Execution Options\sethc.exe*'
            - '*\CurrentVersion\Image File Execution Options\utilman.exe*'
            - '*\CurrentVersion\Image File Execution Options\osk.exe*'
            - '*\CurrentVersion\Image File Execution Options\magnify.exe*'
            - '*\CurrentVersion\Image File Execution Options\narrator.exe*'
            - '*\CurrentVersion\Image File Execution Options\displayswitch.exe*'
    condition: selection
falsepositives:
    - Penetration Tests
level: high
        

```





### es-qs
    
```
CommandLine.keyword:(*\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\sethc.exe* OR *\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\utilman.exe* OR *\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\osk.exe* OR *\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\magnify.exe* OR *\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\narrator.exe* OR *\\\\CurrentVersion\\\\Image\\ File\\ Execution\\ Options\\\\displayswitch.exe*)
```


### xpack-watcher
    
```
curl -s -XPUT -H \'Content-Type: application/json\' --data-binary @- localhost:9200/_watcher/watch/Suspicious-Debugger-Registration-Cmdline <<EOF\n{\n  "metadata": {\n    "title": "Suspicious Debugger Registration Cmdline",\n    "description": "Detects the registration of a debugger for a program that is available in the logon screen (sticky key backdoor).",\n    "tags": [\n      "attack.persistence",\n      "attack.privilege_escalation",\n      "attack.t1015"\n    ],\n    "query": "CommandLine.keyword:(*\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\sethc.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\utilman.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\osk.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\magnify.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\narrator.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\displayswitch.exe*)"\n  },\n  "trigger": {\n    "schedule": {\n      "interval": "30m"\n    }\n  },\n  "input": {\n    "search": {\n      "request": {\n        "body": {\n          "size": 0,\n          "query": {\n            "bool": {\n              "must": [\n                {\n                  "query_string": {\n                    "query": "CommandLine.keyword:(*\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\sethc.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\utilman.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\osk.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\magnify.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\narrator.exe* OR *\\\\\\\\CurrentVersion\\\\\\\\Image\\\\ File\\\\ Execution\\\\ Options\\\\\\\\displayswitch.exe*)",\n                    "analyze_wildcard": true\n                  }\n                }\n              ],\n              "filter": {\n                "range": {\n                  "timestamp": {\n                    "gte": "now-30m/m"\n                  }\n                }\n              }\n            }\n          }\n        },\n        "indices": []\n      }\n    }\n  },\n  "condition": {\n    "compare": {\n      "ctx.payload.hits.total": {\n        "not_eq": 0\n      }\n    }\n  },\n  "actions": {\n    "send_email": {\n      "email": {\n        "to": "root@localhost",\n        "subject": "Sigma Rule \'Suspicious Debugger Registration Cmdline\'",\n        "body": "Hits:\\n{{#ctx.payload.hits.hits}}{{_source}}\\n================================================================================\\n{{/ctx.payload.hits.hits}}",\n        "attachments": {\n          "data.json": {\n            "data": {\n              "format": "json"\n            }\n          }\n        }\n      }\n    }\n  }\n}\nEOF\n
```


### graylog
    
```
CommandLine.keyword:(*\\\\CurrentVersion\\\\Image File Execution Options\\\\sethc.exe* *\\\\CurrentVersion\\\\Image File Execution Options\\\\utilman.exe* *\\\\CurrentVersion\\\\Image File Execution Options\\\\osk.exe* *\\\\CurrentVersion\\\\Image File Execution Options\\\\magnify.exe* *\\\\CurrentVersion\\\\Image File Execution Options\\\\narrator.exe* *\\\\CurrentVersion\\\\Image File Execution Options\\\\displayswitch.exe*)
```


### splunk
    
```
(CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\sethc.exe*" OR CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\utilman.exe*" OR CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\osk.exe*" OR CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\magnify.exe*" OR CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\narrator.exe*" OR CommandLine="*\\\\CurrentVersion\\\\Image File Execution Options\\\\displayswitch.exe*")
```


### logpoint
    
```
(event_id="1" CommandLine IN ["*\\\\CurrentVersion\\\\Image File Execution Options\\\\sethc.exe*", "*\\\\CurrentVersion\\\\Image File Execution Options\\\\utilman.exe*", "*\\\\CurrentVersion\\\\Image File Execution Options\\\\osk.exe*", "*\\\\CurrentVersion\\\\Image File Execution Options\\\\magnify.exe*", "*\\\\CurrentVersion\\\\Image File Execution Options\\\\narrator.exe*", "*\\\\CurrentVersion\\\\Image File Execution Options\\\\displayswitch.exe*"])
```


### grep
    
```
grep -P '^(?:.*.*\\CurrentVersion\\Image File Execution Options\\sethc\\.exe.*|.*.*\\CurrentVersion\\Image File Execution Options\\utilman\\.exe.*|.*.*\\CurrentVersion\\Image File Execution Options\\osk\\.exe.*|.*.*\\CurrentVersion\\Image File Execution Options\\magnify\\.exe.*|.*.*\\CurrentVersion\\Image File Execution Options\\narrator\\.exe.*|.*.*\\CurrentVersion\\Image File Execution Options\\displayswitch\\.exe.*)'
```



