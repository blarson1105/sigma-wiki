# Rule Creation

Sigma is a very flexible standard with many optional fields. This guide will help you create a Sigma rule that aligns with the other community rules in our repository. 

## Rule Template

The best way is to use an existing rule that gets close to what you plan like to write. 

Make sure that these fields are set in a rule that you would like to push to our public repository:

```yaml
title: a short capitalized title with less than 50 characters
id: generate one here https://www.uuidgenerator.net/version4
status: experimental
description: A description of what your rule is meant to detect 
references:
    - A list of all references that can help a reader or analyst understand the meaning of a triggered rule
tags:
    - attack.execution  # example MITRE ATT&CK category
    - attack.t1059      # example MITRE ATT&CK technique id
    - car.2014-04-003   # example CAR id
author: Michael Haag, Florian Roth, Markus Neis  # example, a list of authors
date: 2018/04/06  # Rule date
logsource:                      # important for the field mapping in predefined or your additional config files
    category: process_creation  # In this example we choose the category 'process_creation'
    product: windows            # the respective product
detection:
    selection:
        FieldName: 'StringValue'
        FieldName: IntegerValue
        FieldName|modifier: 'Value'
    condition: selection
fields:
    - fields in the log source that are important to investigate further
falsepositives:
    - describe possible false positive conditions to help the analysts in their investigation
level: one of four levels (low, medium, high, critical)
```

## Common Pitfalls

### Title

- Don't use a prefix in the title like "Detects .."
- Use a short title with less than 50 characters as an alert name
- Save any explanation for the description

Bad Examples:
- Detects a process execution in a Windows folder that shouldn't contain executables (unnecessary prefix, too long, all lower case, contains an explanation)
- Detects process injection (unnecessary prefix, too general, lower case first characters)

Good Examples:
- Process Injection Using Iexplore.exe
- Suspicious PowerShell Cmdline with JAB
- Certutil Lolbin Decode Use

### ID

No known pitfalls. We use the optional field `id` in our repo to provide a unique identifier that never changes, while all other field values of that rule could change over time. 

### Status

Every new rule has the status of `experimental`. It gets the status `stable` after months of productive use and without any or positive feedback from the community. 

### Description

No known pitfalls. Try to describe as good as possible what it means if that rule triggers. 

### References 

The values must be a list. 

Use links to web pages or documents only. Don't link to EVTX files, PCAPs or other raw content.

### Author

- The author field is a string, not a list
- Combine multiple authors separated with a comma
- If you use a special character like `@` for a twitter handle, you have to use upper ticks, e.g. `author: '@cyb3rops'`

### Date

We use the optional field date in our public rules to show the creation date of the rule without requiring a git-log. Changing a once in the `master` branch published rule, it gets an additional field named `modified` to indicate a modification of the initial rule.

We use the format `YYYY/MM/DD` or `%Y/%m/%d` as Python's strftime directive.

### Tags

In our public ruleset, we use tags from [MITRE ATT&CK](https://attack.mitre.org/) and [CAR](https://mitre-attack.github.io/caret/#/). You can find appropriate links following the respective links. 

- Use lower-case tags only
- Replace space ` ` or hyphens `-` with an underscore `_`

### Log Source

This is a more difficult section. There are two options: 

1. The log source already exists
2. There is not a single rule in our repo for that log source

In case 1 please use one of these rules as a template. In case 2, check the existing rules in the different folders to get a feeling for the use of the three identifiers that you can use in this section: 

- product (e.g. linux, windows, cisco)
- service (e.g. sysmon, ldapd, dhcp)
- category (e.g. process_creation)

Note that these identifiers are used in the config files in `./tools/config` used by `sigmac` to map a specific log source's fields to the fields that you use in your rule. If you create a new log source, it would be great if you could add appropriate mapping in all the current config files for the different backends (qradar, helk, Splunk-windows etc.). Otherwise other users or we have maintainers have to do that.

### Detection

The detection section is very flexible but we see common errors or styling issues in this section that require a reword by us maintainers. 

1. If your list consists of a single element, don't use a list (see examples below)
2. Use only lowercase identifiers
3. Put comments on lines if you like to (use 2 spaces to separate the expression from your comment) 
4. Don't use regular expressions unless you really have to (e.g. instead of `Filename|re: '.*\\.exe'` use `Filename|endswith: '\.exe').
5. In new sources use the field names as they appear in the log source, remove spaces and keep hyphens (e.g. `SAM User Account` becomes `SAMUserAccount`)
6. Don't use SIEM specific logic in your condition
7. Test your rule before you commit them (we often see broken conditions)

### Fields 

These are the fields that are very helpful in the evaluation of a certain event. For example, it is helpful to know the parent process of a process that contains suspicious strings in its command line parameters. 

These fields could be extracted automatically and presented to the analyst in order to speed up the analysis. 

### False Positive

Think about possible false positive conditions that could also trigger the rule. This list should contain useful hints for an analyst. E.g. the comment "often used in penetration tests" could induce the analyst to ask the respective department first before ringing the alarm bells. 

### Level

The four existing levels could further be divided into two categories. 

1. Rules that have informative character and should be displayed in a list or bar chart (low, medium)
2. Rules that should trigger a dedicated alert (high, critical) 

Apply the following guidelines when setting a level: 
- Rules of level `critical` should never trigger a false positive
- Rules of level `high` trigger alerts that have to be reviewed manually
- Rules of level `high` and `critical` indicate an incident (if not a false positive)
- Rules of level `low` and `medium` indicate suspicious activity and policy violations
- Rules of level `low` have informative character and are often used for compliance or correlation purposes

## External Guides

The following links lead to web pages that explain the rule creation process with screenshots or certain aspects of the expression language.

- [How to Write Sigma Rules](https://www.nextron-systems.com/2018/02/10/write-sigma-rules/)
- [Introducing Generic Log Sources in Sigma](https://patzke.org/introducing-generic-log-sources-in-sigma.html)
- [A Guide to Generic Log Sources in Sigma](https://patzke.org/a-guide-to-generic-log-sources-in-sigma.html)
- [Introducing Sigma Value Modifiers](https://patzke.org/introducing-sigma-value-modifiers.html)
- [Developing Value Modifiers](https://patzke.org/developing-value-modifiers.html)
