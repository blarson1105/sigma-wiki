## Purpose

This page defines some standardized tags that can be used to categorize Sigma rules.

## Namespaces

* attack: Categorization according to [MITRE ATT&CK](https://attack.mitre.org)
* car: Link to the corresponding [MITRE Cyber Analytics Repository (CAR)](https://car.mitre.org/)

### Namespace: attack

* t*1234*: Refers to a [technique](https://attack.mitre.org/wiki/All_Techniques)
* g*1234*: Refers to a [group](https://attack.mitre.org/wiki/Groups)
* s*1234*: Refers to [software](https://attack.mitre.org/wiki/Software)

Tactics:
* initial_access: [Initial Access](https://attack.mitre.org/wiki/Initial_Access)
* execution: [Execution](https://attack.mitre.org/wiki/Execution)
* persistence: [Persistence](https://attack.mitre.org/wiki/Persistence)
* privilege_escalation: [Privilege Escalation](https://attack.mitre.org/wiki/Privilege_Escalation)
* defense_evasion: [Defense Evasion](https://attack.mitre.org/wiki/Defense_Evasion)
* credential_access: [Credential Access](https://attack.mitre.org/wiki/Credential_Access)
* discovery: [Discovery](https://attack.mitre.org/wiki/Discovery)
* lateral_movement: [Lateral_Movement](https://attack.mitre.org/wiki/Lateral_Movement)
* collection: [Collection](https://attack.mitre.org/wiki/Collection)
* exfiltration: [Exfiltration](https://attack.mitre.org/wiki/Exfiltration)
* c2: [Command and Control](https://attack.mitre.org/wiki/Command_and_Control)
+ impact: [Impact](https://attack.mitre.org/tactics/TA0040/)

### Namespace: car

Use the CAR tag from the [analytics repository](https://car.mitre.org/analytics/) without the prepending `CAR-`. Example tag: `car.2016-04-005`.