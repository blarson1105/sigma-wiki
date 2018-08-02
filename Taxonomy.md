This page defines field names and log sources that should be used to ensure sharable rules.

## Log Sources

### Generic

* `category: process_creation`: Defines a process creation event

### Specific

* `product: windows`: Windows Operating System logs
  * `service: security`: Windows Security Event Log
  * `service: security`: Windows Security Event Log
  * `service: sysmon`: Event Logs created by Sysmon. Some may be covered by [generic log sources](#generic)

## Field Names

### Generic Log Source: process_creation
