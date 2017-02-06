# Specification

The rules consist of a few required sections and several optional ones.

```
title
status [optional]
description [optional]
reference [optional]
detection
  {search-identifier} [optional]
    {string-list} [optional]
    {field: value} [optional]
  ...
  timeframe [optional]
  condition
falsepositives [optional]
level [optional]
```

## Title

A brief title for the rule that should contain what the rules is supposed to detect (max. 256 characters)

## Status

Declares the status of the rule:

- stable: the rule is considered as stable and may be used in production systems or dashboards.
- test: an almost stable rule that possibly could require some fine tuning.
- experimental: an experimental rule that could lead to false results or be noisy, but could also identify interesting
  events.

## Description

A short description of the rule and the malicious activity that can be detected (max. 65,535 characters)

## Detection

A set of search-identifiers that represent searches on log data

## Search-Identifier

A definition that can consist of two different data structures - lists and maps.

### Lists

The lists contain strings that are applied to the full log message and are logically linked with an 'OR'y.

Example:

```
detection:
  keywords:
    - EVILSERVICE
    - svchost.exe -n evil
```

This example results in the following search expression:

```
EvilService OR "svchost.exe -n evil"
```

### Maps

Maps (or dictionaries) consist of key/value pairs, in which the key is a field in the log data and the value a string or integer value. Lists of maps are joined with a logical 'OR'. All elements of a map are joined with a logical 'AND'.

Examples:

```
detection:
  selection:
    - EventLog: Security
      EventID:
        - 517
        - 1102
condition: selection
```

Splunk Search (implicit AND):

```
EventLog=Security ( EventID=517 OR EventID=1102 )
```

### TimeFrame

A relative time frame definition using the typical abbreviations for day, hour, minute, second.

Examples:

```
15s
30m
12h
7d
3M
```

Note: The time frame is often a manual setting that has to be defined within the SIEM system and is not part of the generated query.

## Condition

The condition is the most complex part of the specification and will be subject to change over time and arising requirements. In the first release it will support the following expressions.

- Logical AND/OR

  ```keywords1 or keywords2```

- X of search-identifier

  ```1 of keywords```

  Same as just 'keywords' if keywords are dafined in a list. X may be:

  - 1 (logical or across alternatives)
  - all (logical and across alternatives)

- Negation with 'not'

  ```keywords and not filters```

- Pipe

  ```expression1 | expression2```

  A pipe indicates that the result of search *expression1* is:

  - filtered by search *expression2* (equivalent to logical *and*)
  - aggregated by aggregation *expression2* and possibly compared with a value

  The first expression must be a search expression that may be followed by an arbitrary number of filtering search
  expressions and finally followed by an aggregation expression with a condition.

- Brackets

  ```selection1 and (keywords1 or keywords2)```

- Aggregation expression

  agg-function(agg-field) [ by group-field ] comparison-op value

  agg-function may be:

  - count
  - distcount (count distinct values)
  - min
  - max
  - avg
  - sum

  Example: ```count(UserName) by SourceWorkstation > 3```

  The comparison operates on the result values of all aggregation groups or buckets. It is sufficient when one group matches
  the condition.

Operator Precedence (least to most binding)

- |
- or
- and
- not
- x of search-identifier
- ( expression )

## FalsePositives

A list of known false positives that may occur.

## Level

A score between 0 and 100 to define the degree of likelyhood that generated events are actually incidents.

A rough guideline would be:

- 20 : Interesting event but less likely that it's actually an incident. A security analyst has to review the events and spot anomalies or suspcious indicators. Use this in a dashboard panel, maybe in form of a chart.
- 40 : Interesting event, that shouldn't trigger too often. A security analyst has to review the events and spot anomalies or suspcious indicators. List the events in a dashboard panel for manual review.
- 60 : Relevant event that should be reviewed manually on a more frequent basis. A security analyst has to review the events and spot anomalies or suspcious indicators. List the events in a dashboard panel for manual review.
- 80 : Relevant event that should trigger an internal alert and has to be reviewed immediately.
- 100 : Highly relevant event that triggers an internal alert and causes external notifications (eMail, SMS, ticket). Events are clear matches with no known false positives.    

## Placeholders

/ not yet defined /
