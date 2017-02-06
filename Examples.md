# Examples

A very simple example is the 'Eventlog Cleared' signature. I describes the following idea:

* From the Windows Eventlog 'Security'
* Select all events with Event ID 517 OR 1102

```
title: Eventlog Cleared
description: Some threat groups tend to delete the local 'Security'' Eventlog using certain utitlities
detection:
    selection:
        - EventLog: Security
          EventID:
              - 517
              - 1102
    condition: selection
falsepositives:
    - Rollout of log collection agents (the setup routine often includes a reset of the local Eventlog)
    - System provisioning (system reset before the golden image creation)
level: 70
```
