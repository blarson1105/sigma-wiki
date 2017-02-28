# Which SIEM / log management systems will be supported?

Sigma is a generic signature format that can be converted manually or automatically into search expressions. The first search language that will be supported in automatic conversion using the Sigma converter tool named "sigmac" is the ElasticSearch query language (ES Query String). The next language on our roadmap is the Splunk query language. We welcome support for various other languages and systems as e.g. Ariel Query Language (AQL, QRadar). 

# Why using YAML?

It is easy to read and write, flexible and provides all needed language elements. However it lacks a standardized schema language as XML provides it. 

# Isn't Sigma the same as OSSEC?

Sigma aims at being a generic signature format for all kinds of log data, whereas OSSEC covers host based detection very well in its own specific XML rule base. We could definitely convert the OSSEC rule base licensed under the GPL and provide those as rules in Sigma format so that the detections become available in any SIEM system. 
     