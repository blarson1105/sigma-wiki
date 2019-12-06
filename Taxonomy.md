This page defines field names and log sources that should be used to ensure sharable rules.

## Log Sources

### Generic

#### Process Creation Events

Process creation events can be defined with the generic log source category *process_creation*. The event scope can be further restricted with *product*. Example for a process creation event log source restricted to Windows:

```
category: process_creation
product: windows
```

The field names follow the field names used in [Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) events:

|Field Name|Example Value|Comment|
|-------|---|---|
|UtcTime|2019-03-02 08:51:00.008|(useless)|
|ProcessGuid|{c1b49677-43f4-5c7a-0000-0010d3dd8044}|(useless)|
|ProcessId|1028|   |
|Image|C:\Program Files (x86)\Google\Update\GoogleUpdate.exe|   |
|FileVersion|1.3.28.13|   |
|Description|Google Installer|   |
|Product|Google Update|   |
|Company|Google Inc.|   |
|CommandLine|"C:\Program Files (x86)\Google\Update\GoogleUpdate.exe" /ua /installsource scheduler|   |
|CurrentDirectory|C:\Windows\system32\|   |
|User|NT AUTHORITY\SYSTEM|   |
|LogonGuid|{c1b49677-3fb9-5c09-0000-0020e7030000}|(useless)|
|LogonId|0x3e7|   |
|TerminalSessionId|0|   |
|IntegrityLevel|System|   |
|Hashes|SHA1=5B2EA705524EF2E47B79FBEFC70F7AAFF474C36D|   |
|ParentProcessGuid|{c1b49677-6b43-5c78-0000-00107fb77544}|(useless)|
|ParentProcessId|1724|   |
|ParentImage|C:\Windows\System32\taskeng.exe|   |
|ParentCommandLine|taskeng.exe {88F94E5C-5DC3-4606-AEFA-BDCA976D6113} S-1-5-18:NT AUTHORITY\System:Service:|   |

### Specific

* `product: windows`: Windows Operating System logs. The naming of Windows Eventlog attributes is used in Sigma rules.
  * `service: security`: Windows Security Event Log. Some may be covered by [generic log sources](#generic).
  * `service: system`: Windows System Event Log
  * `service: sysmon`: Event Logs created by Sysmon. Some may be covered by [generic log sources](#generic).
  * `service: taskscheduler`
  * `service: wmi`
  * `service: application`
  * `service: dns-server`
  * `service: driver-framework`
  * `service: powershell`
  * `service: powershell-classic`: ???
* `product: linux`: Linux log files
  * `service: auth`: Linux authentication logs. Usually */var/log/auth.log*.
  * `service: auditd`: Linux audit logs
  * `service: clamav`: ClamAV logs
* `product: apache`: Apache httpd logs
  * `service: access`: Access logs
  * `service: error`: Error logs
* `category: proxy`
  * Field Names: [W3C Extended Log File Format](https://www.w3.org/TR/WD-logfile.html)
* `category: firewall`
  * Field Names:
    * `src_ip`, `src_port`, `dst_ip`, `dst_port`, `username`
* `category: dns`
* `category: webserver`