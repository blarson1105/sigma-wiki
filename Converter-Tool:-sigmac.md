# Configuration File

The configuration file contains mappings for the target environments:

* between generic Sigma field names and these used in the target
* between log source identifiers from Sigma and index names from target

The mappings are configured in a YAML file with the following format:

```
fieldnames:
  sigma_fieldname: target_fieldname
logsources:
  sigma_logsource: target_indexname
```
# Addition of Target Formats
Addition of a target format is done by development of a backend class. A backend class gets a parse tree as input and must translate parse tree nodes into the target format.

## Translation Process

1. Parsing YAML
2. Parsing of Condition
3. Internal representation of condition as parse tree
4. Attachment of definitions into corresponding parse tree nodes
5. Translation of parse tree into target format (backend classes)

## Parse Tree Node Types
tbd