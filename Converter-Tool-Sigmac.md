# Configuration File

The configuration file contains mappings for the target environments:

* between generic Sigma field names and these used in the target
* between log source identifiers from Sigma and...
  * ...index names from target
  * ...conditions that should be added to generated expression (e.g. EventLog: Microsoft-Windows-Sysmon) with AND.

The mappings are configured in a YAML file with the following format:

```
fieldmappings:
  sigma_fieldname: target_fieldname
logsources:
  sigma_logsource:
    category: ...
    product: ...
    service: ...
    index:
      - target_indexname1
      - target_indexname2
    conditions:
      field1: value1
      field2: value2
logsourcemerging: and/or
```

Field mappings in the *fieldmappings* section are simple *source_field_name:target_field_name* mappings. Use the *fieldlist* backend to determine all field names used by rules. Example:

```
$ tools/sigmac.py -r -t fieldlist rules/windows/ 2>/dev/null | sort -u
AccessMask
CallTrace
CommandLine
[...]
TicketOptions
Type
```

Each log source definition must contain at least one category, product or service element that corresponds to the same fields in the logsources part of sigma rules. If more than one field is given, all must match (AND).

The *index* field can contain a string or a list of strings. They a converted to the target expression language in a way that the rule is searched in all given index patterns.

The conditions part can be used to define *field: value* conditions if only a subset of the given indices is relevant. All fields are linked with logical AND and the resulting expression is also lined with AND against the expression generated from the sigma rule.

Example: a logstash configuration passes all Windows logs in one index. For Sysmon only events that match *EventLog:"Microsoft-Windows-Sysmon" are relevant. The config looks as follows:

```
...
logsources:
  sysmon:
    product: sysmon
    index: logstash-windows-*
    conditions:
      EventLog: Microsoft-Windows-Sysmon
...
```

If multiple log source definitions match, the result is merged from all matching rules. The parameter *logsourcemerging* determines how conditions are merged. The following methods are supported:

* and (default): merge all conditions with logical AND.
* or: merge all conditions with logical OR.

# Addition of Target Formats
Addition of a target format is done by development of a backend class. A backend class gets a parse tree as input and must translate parse tree nodes into the target format.

## Translation Process

1. Parsing YAML
2. Parsing of Condition
3. Internal representation of condition as parse tree
4. Attachment of definitions into corresponding parse tree nodes
5. Translation of field and log source identifiers into target names
6. Translation of parse tree into target format (backend classes)

## Parse Tree Node Types
tbd