---
title: "CAR-2020-11-011: Registry Edit from Screensaver"
layout: analytic
submission_date: 2020/11/30
information_domain: Host
subtypes: Process
analytic_type: TTP
contributors: Olaf Hartong
applicable_platforms: Windows
---

Adversaries may use screensaver files to run malicious code. This analytic triggers on suspicious edits to the screensaver registry keys, which dictate which .scr file the screensaver runs.


### ATT&CK Detection

|Technique|Subtechnique(s)|Tactic(s)|Level of Coverage|
|---|---|---|---|
|[Event Triggered Execution](https://attack.mitre.org/techniques/T1546/)|[Screensaver](https://attack.mitre.org/techniques/T1546/002/)|[Persistence](https://attack.mitre.org/tactics/TA0003/), [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/)|High|

### Data Model References

|Object|Action|Field|
|---|---|---|
|[registry](/data_model/registry) | [edit](/data_model/registry#edit) | [key](/data_model/registry#key) |
|[registry](/data_model/registry) | [add](/data_model/registry#add) | [key](/data_model/registry#key) |


### Applicable Sensors

- [Autoruns_13.98](/sensors/Autoruns_13.98)
- [osquery_4.1.2](/sensors/osquery_4.1.2)
- [osquery_4.6.0](/sensors/osquery_4.6.0)
- [Sysmon_10.4](/sensors/sysmon_10.4)
- [Sysmon_11.0](/sensors/sysmon_11.0)
- [Sysmon_13](/sensors/sysmon_13)

### Implementations

#### Pseudocode - Screensaver (Pseudocode, CAR native)


This is a pseudocode representation of the below splunk search.


```
reg_events = search Registry:add or Registry:edit
scr_reg_events = filter processes where (
  key="*\\Software\\Policies\\Microsoft\\Windows\\Control Panel\\Desktop\\SCRNSAVE.EXE" AND
output scr_reg_events
```


#### Splunk Search - Screensaver (Splunk, Sysmon native)


looks creations of edits of the SCRNSAVE.exe registry key


```
index=your_sysmon_index (EventCode=12 OR EventCode=13 OR EventCode=14) TargetObject="*\\Software\\Policies\\Microsoft\\Windows\\Control Panel\\Desktop\\SCRNSAVE.EXE"
```


#### LogPoint Search - Screensaver (Logpoint, LogPoint native)


looks creations of edits of the SCRNSAVE.exe registry key


```
norm_id=WindowsSysmon event_id IN [12, 13, 14] target_object="*\Software\Policies\Microsoft\Windows\Control Panel\Desktop\SCRNSAVE.EXE"
```




