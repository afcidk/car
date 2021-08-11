---
title: "CAR-2021-01-003: Clearing Windows Logs with Wevtutil"
layout: analytic
submission_date: 2020/12/02
information_domain: Host
subtypes: Process
analytic_type: TTP
contributors: Cyware Labs
applicable_platforms: Windows
---

In an attempt to clear traces after compromising a machine, threat actors often try to clear Windows Event logs. This is often done using “wevtutil”, a legitimate tool provided by Microsoft. This action interferes with event collection and notification, and may lead to a security event going undetected, thereby potentially leading to further compromise of the network.


### ATT&CK Detection

|Technique|Subtechnique(s)|Tactic(s)|Level of Coverage|
|---|---|---|---|
|[Indicator Removal on Host](https://attack.mitre.org/techniques/T1070/)|[Clear Windows Event Logs](https://attack.mitre.org/techniques/T1070/001/)|[Defense Evasion](https://attack.mitre.org/tactics/TA0005/)|Low|

### Data Model References

|Object|Action|Field|
|---|---|---|
|[process](/data_model/process) | [create](/data_model/process#create) | [command_line](/data_model/process#command_line) |


### Applicable Sensors

- [osquery_4.1.2](/sensors/osquery_4.1.2)
- [osquery_4.6.0](/sensors/osquery_4.6.0)
- [Sysmon_10.4](/sensors/sysmon_10.4)
- [Sysmon_11.0](/sensors/sysmon_11.0)
- [Sysmon_13](/sensors/sysmon_13)

### Implementations

#### Splunk search - Detecting log clearing with wevtutil (Splunk, Sysmon native)


This search query looks for an instance where wevtutil is invoked along with a command that may cause the system to remove Windows Event logs.


```
index=__your_sysmon_index__ sourcetype= __your__windows__sysmon__sourcetype EventCode=1 Image=*wevtutil* CommandLine=*cl* (CommandLine=*System* OR CommandLine=*Security* OR CommandLine=*Setup* OR CommandLine=*Application*)
```




