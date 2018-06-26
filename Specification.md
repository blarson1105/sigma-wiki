# Structure

The rules consist of a few required sections and several optional ones.

```
title
status [optional]
description [optional]
author [optional]
references [optional]
logsource
   category [optional]
   product [optional]
   service [optional]
   definition [optional]
   ...
detection
   {search-identifier} [optional]
      {string-list} [optional]
      {field: value} [optional]
   ...
   timeframe [optional]
   condition
fields [optional]
falsepositives [optional]
level [optional]
tags [optional]
...
[arbitrary custom fields]
```

# Schema

## Rx YAML
```
type: //rec
required:
    title:
        type: //str
        length:
            min: 1
            max: 256
    logsource:
        type: //rec
        optional:
            category: //str
            product: //str
            service: //str
            definition: //str
    detection:
        type: //rec
        required:
            condition:
                type: //any
                of:
                    - type: //str
                    - type: //arr
                      contents: //str
                      length:
                          min: 2
        optional:
            timeframe: //str
        rest:
            type: //any
            of:
                - type: //arr
                  contents: //str
                - type: //map
                  values:
                      type: //any
                      of:
                          - type: //str
                          - type: //arr
                            contents: //str
                            length:
                                min: 2
optional:
    status:
        type: //any
        of:
            - type: //str
              value: stable
            - type: //str
              value: testing
            - type: //str
              value: experimental
    description: //str
    author: //str
    references:
        type: //arr
        contents: //str
    fields:
        type: //arr
        contents: //str
    falsepositives:
        type: //any
        of:
            - type: //str
            - type: //arr
              contents: //str
              length:
                  min: 2
    level:
        type: //any
        of:
            - type: //str
              value: low
            - type: //str
              value: medium
            - type: //str
              value: high
            - type: //str
              value: critical
    tags:
        type: //arr
        contents: //str
rest: //any
```

## Image

![sigma_schema](https://github.com/Neo23x0/sigma/blob/master/images/Sigma_Schema.png)

# Components

## Title

A brief title for the rule that should contain what the rules is supposed to detect (max. 256 characters)

## Status (optional)

Declares the status of the rule:

- stable: the rule is considered as stable and may be used in production systems or dashboards.
- test: an almost stable rule that possibly could require some fine tuning.
- experimental: an experimental rule that could lead to false results or be noisy, but could also identify interesting
  events.

## Description (optional)

A short description of the rule and the malicious activity that can be detected (max. 65,535 characters)

## Author (optional)

Creator of the rule.

## References (optional)

References to the source that the rule was derived from. These could be blog articles, technical papers, presentations or even tweets.

## Log Source

This section describes the log data on which the detection is meant to be applied to. It describes the log source, the platform, the application and the type that is required in detection. 

It consists of three attributes that are evaluated automatically by the converters and an arbitrary number of optional elements. We recommend using a "definition" value in cases in which further explication is necessary.    

* category - examples: firewall, web, antivirus
* product - examples: windows, apache, check point fw1
* service - examples: sshd, applocker

The "category" value is used to select all log files written by a certain group of products, like firewalls or web server logs. The automatic conversion will use the keyword as a selector for multiple indices. 

The "product" value is used to select all log outputs of a certain product, e.g. all Windows Eventlog types including "Security", "System", "Application" and the new log types like "AppLocker" and "Windows Defender".

Use the "service" value to select only a subset of a product's logs, like the "sshd" on Linux or the "Security" Eventlog on Windows systems. 

The "definition" can be used to describe the log source, including some information on the log verbosity level or configurations that have to be applied. It is not automatically evaluated by the converters but gives useful advice to readers on how to configure the source to provide the necessary events used in the detection. 

You can use the values of 'category, 'product' and 'service' to point the converters to a certain index. You could define in the configuration files that the category 'firewall' converts to ```( index=fw1* OR index=asa* )``` during Splunk search conversion or the product 'windows' converts to ```"_index":"logstash-windows*"``` in ElasticSearch queries.    

## Detection

A set of search-identifiers that represent searches on log data

## Search-Identifier

A definition that can consist of two different data structures - lists and maps.

### General 

* All values are treated as case-insensitive strings
* You can use wildcard characters '*' and '?' in strings
* Regular expressions are case-sensitive by default
* You don't have to escape characters except the string quotation marks ```'```

### Lists

The lists contain strings that are applied to the full log message and are linked with a logical 'OR'.

Example: Matches on 'EvilService' **or** 'svchost.exe -n evil'

```
detection:
  keywords:
    - EVILSERVICE
    - svchost.exe -n evil
```

### Maps

Maps (or dictionaries) consist of key/value pairs, in which the key is a field in the log data and the value a string or integer value. Lists of maps are joined with a logical 'OR'. All elements of a map are joined with a logical 'AND'.

Examples:

Matches on Eventlog 'Security' **and** ( Event ID 517 **or** Event ID 1102 ) 

```
detection:
  selection:
    - EventLog: Security
      EventID:
        - 517
        - 1102
condition: selection
```

Matches on Eventlog 'Security' **and** Event ID 4679 **and** TicketOptions 0x40810000 **and** TicketEncryption 0x17 

```
detection:
   selection:
      - EventLog: Security
        EventID: 4769
        TicketOptions: '0x40810000'
        TicketEncryption: '0x17'
condition: selection
```

### Special Field Values

There are special field values that can be used.

* An empty value is defined with ```''```
* A null value is defined with ```(null)```
* An arbitrary value except null or empty is defined with ```(any)```

The application of these values depends on the target SIEM system.  

### TimeFrame

A relative time frame definition using the typical abbreviations for day, hour, minute, second.

Examples:

```
15s  (15 seconds)
30m  (30 minutes)
12h  (12 hours)
7d   (7 days)
3M   (3 months)
```
The time frame is defined in the *timeframe* attribute of the *detection* section.

Note: The time frame is often a manual setting that has to be defined within the SIEM system and is not part of the generated query.

## Condition

The condition is the most complex part of the specification and will be subject to change over time and arising requirements. In the first release it will support the following expressions.

- Logical AND/OR

  ```keywords1 or keywords2```

- 1/all of search-identifier

  Same as just 'keywords' if keywords are defined in a list. X may be:

  - 1 (logical or across alternatives)
  - all (logical and across alternatives)

  Example: `all of keywords` means that all items of the list keywords must appear, instead of the default behaviour of any of the listed items.

- 1/all of them

  Logical OR (`1 of them`) or AND (`all of them`) across all defined search identifiers. The search identifiers
  themselves are logically linked with their default behaviour for maps (AND) and lists (OR).

  Example: `1 of them` means that one of the defined search identifiers must appear.

- 1/all of search-identifier-pattern

  Same as *1/all of them*, but restricted to matching search identifiers. Matching is done with * wildcards (any number of characters) at arbitrary positions in the pattern.

  Examples:
  - `1 of selection* and keywords`
  - `any of selection* and not filters`

- Negation with 'not'

  ```keywords and not filters```

- Pipe

  ```search_expression | aggregation_expression```

  A pipe indicates that the result of *search_expression* is aggregated by *aggregation_expression* and possibly
  compared with a value

  The first expression must be a search expression that is followed by an aggregation expression with a condition.

- Brackets

  ```selection1 and (keywords1 or keywords2)```

- Aggregation expression

  agg-function(agg-field) [ by group-field ] comparison-op value

  agg-function may be:

  - count
  - min
  - max
  - avg
  - sum

  All aggregation functions except count require a field name as parameter. The count aggregation counts all matching events if no field name is given. With field name it counts the distinct values in this field.

  Example: ```count(UserName) by SourceWorkstation > 3```

  This comparison counts distinct user names grouped by SourceWorkstations.


- Near aggregation expression

  near *search-id-1* [ [ and *search-id-2* | and not *search-id-3* ] ... ]

  This expression generates (if supported by the target system and backend) a query that recognizes *search_expression* (primary event) if the given conditions are or are not in the temporal context of the primary event within the given time frame.

Operator Precedence (least to most binding)

- |
- or
- and
- not
- x of search-identifier
- ( expression )

If multiple conditions are given, they are logically linked with OR.

## Fields

A list of log fields that could be interesting in further analysis of the event and should be displayed to the analyst.

## FalsePositives

A list of known false positives that may occur.

## Level

The level contains one of four string values. It serves as a guideline for using the signature and a way to deliver matching events.

- low : Interesting event but less likely that it's actually an incident. A security analyst has to review the events and spot anomalies or suspicious indicators. Use this in a dashboard panel, maybe in form of a chart.
- medium : Relevant event that should be reviewed manually on a more frequent basis. A security analyst has to review the events and spot anomalies or suspicious indicators. List the events in a dashboard panel for manual review.
- high : Relevant event that should trigger an internal alert and has to be reviewed as quickly as possible.
- critical : Highly relevant event that triggers an internal alert and causes external notifications (eMail, SMS, ticket). Events are clear matches with no known false positives.

## Tags

A Sigma rule may be categorized with tags. Tags should generally follow this syntax:

* Character set: lower-case letters, underscores and hyphens
* no spaces
* Tags are namespaced, the dot is used as separator. e.g. *attack.t1234* refers to technique 1234 in the namespace *attack*. Namespaces may also be nested.
* Keep tags short, e.g. numeric identifiers instead of long sentences.
* If applicable, use [predefined tags](../Tags). Feel free to send pull request or issues with proposals for new tags.

## Placeholders

Placeholders can be used to select a set of elements that can be expanded during conversion. Placeholders map a an identifier to a user defined value that can be set in config files for an automatic replacement during conversion runs. Placeholders are meaningful identifiers that users can easily expand themselves.   

### Examples for placeholders

* ```%Administrators%``` - Administrative user accounts
* ```%JumpServers%``` - Server systems used as jump servers

Some SIEM systems allow using so-called "tags" or "search macros" in queries and can integrate Sigma rules with placeholders directly. Others expand the placeholders values to wildcard strings or regular expressions.

### Examples for conversions

Splunk

* ```AccountName: %Administrators%``` convert to ```tag=Administrators```

Elastic Search 

* ```SourceWorkstation: %JumpServers%``` convert to ```"SourceWorkstation": SRV110[12]```

# Rule Collections

A file may contain multiple YAML documents. These can be complete Sigma rules or *action documents*. A YAML document is handled as action document if the `action` attribute on the top level is set to:

* `global`: Defines YAML content that is merged in all following YAML rule documents in this file. Multiple *global* action documents are accumulated.
** Use case: define metadata and rule parts that are common across all Sigma rules of a collection.
* `reset`: Reset global YAML content defined by *global* action documents.
* `repeat`: Repeat generation of previous rule document with merged data from this YAML document.
** Use case: Small modifications of previously generated rule.

## Example
A common use case is the definition of multiple Sigma rules for similar events like Windows Security EventID 4688 and Sysmon EventID 1. Both are created for process execution events. A Sigma rule collection for this scenario could contain three documents:

1. A global action document that defines common metadata and detection indicators
2. A rule that defines Windows Security log source and EventID 4688
3. A rule that defines Windows Sysmon log source and EventID 1

Alternative solution could be:

1. A global action document that defines common metadata.
2. The Security/4688 rule with all event details.
3. A repeat action document that replaces the logsource and EventID from the rule defined in 2.