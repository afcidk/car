---
title: "CAR-2020-09-005: AppInit DLLs"
layout: analytic
submission_date: 2020/09/10
information_domain: Host
subtypes: Registry
analytic_type: TTP
contributors: Olaf Hartong
applicable_platforms: Windows
---

Adversaries may establish persistence and/or elevate privileges by executing malicious content triggered by AppInit DLLs loaded into processes. Dynamic-link libraries (DLLs) that are specified in the AppInit_DLLs value in the Registry keys `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Windows` or `HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Windows` are loaded by user32.dll into every process that loads user32.dll. These values can be abused to obtain elevated privileges by causing a malicious DLL to be loaded and run in the context of separate processes. Accordingly, this analytic looks for modifications to these registry keys that may be indicative of this type of abuse. 


### ATT&CK Detection

|Technique|Subtechnique(s)|Tactic(s)|Level of Coverage|
|---|---|---|---|
|[Event Triggered Execution](https://attack.mitre.org/techniques/T1546/)|[AppInit DLLs](https://attack.mitre.org/techniques/T1546/010/)|[Persistence](https://attack.mitre.org/tactics/TA0003/), [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/)|Moderate|

### Data Model References

|Object|Action|Field|
|---|---|---|
|[registry](/data_model/registry) | [add](/data_model/registry#add) | [key](/data_model/registry#key) |
|[registry](/data_model/registry) | [remove](/data_model/registry#remove) | [key](/data_model/registry#key) |
|[registry](/data_model/registry) | [edit](/data_model/registry#edit) | [key](/data_model/registry#key) |


### Applicable Sensors

- [Autoruns_13.98](/sensors/Autoruns_13.98)
- [osquery_4.1.2](/sensors/osquery_4.1.2)
- [osquery_4.6.0](/sensors/osquery_4.6.0)
- [Sysmon_10.4](/sensors/sysmon_10.4)
- [Sysmon_11.0](/sensors/sysmon_11.0)
- [Sysmon_13](/sensors/sysmon_13)

### Implementations

#### Pseudocode - AppInit DLL registry modification (Pseudocode, CAR native)


This is a pseudocode representation of the below splunk search.


```
registry_keys = search (Registry:Create AND Registry:Remove AND Registry:Edit) 
appinit_keys = filter registry_keys where (
  key = "*\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls\*" OR 
  key = "*\SOFTWARE\\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls\*""
  )
output clsid_keys
```


#### Splunk search - AppInit DLL registry modification (Splunk, Sysmon native)


This Splunk search looks for any registry keys that were created, deleted, or renamed, as well as any registry values that were set or renamed under the Windows AppInit DLL registry keys.


```
index=__your_sysmon_index__ (EventCode=12 OR EventCode=13 OR EventCode=14) (TargetObject="*\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\Appinit_Dlls\\*" OR TargetObject="*\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\Appinit_Dlls\\*")
```


#### LogPoint search - AppInit DLL registry modification (Logpoint, LogPoint native)


This LogPoint search looks for any registry keys that were created, deleted, or renamed, as well as any registry values that were set or renamed under the Windows AppInit DLL registry keys.


```
norm_id=WindowsSysmon event_id IN [12, 13, 14] target_object IN ["*\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls\*", "*\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Windows\Appinit_Dlls\*"]
```




