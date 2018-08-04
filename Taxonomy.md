This page defines field names and log sources that should be used to ensure sharable rules.

## Log Sources

### Generic

#### Process Creation Events

Process creation events can be defined with the generic log source category *process_creation*. The event scope can be further restricted with *product*. Example for a process creation event log source restricted to Windows:

```
category: process_creation
product: windows
```

The field names follow the Sysmon field naming:

* Image
* CommandLine
* ParentImage
* ParentCommandLine
* Hashes
* User
* IntegrityLevel

### Specific

* `product: windows`: Windows Operating System logs
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
    * `src_ip`, `src_port`, dst_ip`, `dst_port`
* `category: dns`
* `category: webserver`