
## Overview

ELK is a stack of open-source tools used to search, analyze, and visualize large volumes of data in real time. It is composed of three main components:

- Elasticsearch
    
- Logstash
    
- Kibana
    

The main purpose of ELK is centralized logging, security monitoring, and data visualization. It allows security teams to collect logs from multiple sources, analyze them efficiently, and respond to incidents faster.

---

## Elasticsearch

- At its core, Elasticsearch is a database used to store logs such as Windows Event Logs, Syslog, firewall logs, and more.
    
- It allows searching through data using Elasticsearch Query Language (ES|QL).
    
- It supports RESTful APIs and JSON, allowing programmatic interaction with the data.
    
- Data stored in Elasticsearch can also be queried through Kibana’s web interface.
    

---

## Logstash

- Logstash is a server-side data processing pipeline that ingests telemetry from multiple sources, transforms it, and sends it to Elasticsearch.
    
- It can:
    
    - Ingest data from endpoints
        
    - Filter logs and only forward relevant data
        
    - Parse data using existing or custom parsers
        
- Telemetry can be collected using Beats or Elastic Agents. Once data reaches Logstash, it can be filtered or parsed before being stored in Elasticsearch.
    

---

## Beats

Beats are lightweight agents installed on endpoints to collect specific types of data.

Common types of Beats:

- Filebeat (logs)
    
- Metricbeat (metrics)
    
- Packetbeat (network)
    
- Winlogbeat (Windows logs)
    
- Auditbeat (audit data)
    
- Heartbeat (uptime monitoring)
    

You choose the appropriate Beat depending on the type of telemetry you want to collect.

---

## Elastic Agent

- Elastic Agent is a unified agent capable of collecting multiple types of telemetry using a single installation.
    
- This simplifies endpoint management by removing the need to install multiple Beats on the same machine.
    
- For this project, Elastic Agent will be used.
    

---

## Kibana

- Kibana is a visualization and exploration tool that works with Elasticsearch.
    
- It allows users to create visualizations, dashboards, and search logs easily.
    
- Instead of interacting with Elasticsearch through APIs, Kibana provides a web interface for querying.
    

**Main features:**

- Kibana Lens: Build visualizations and dashboards.
    
- Discover: Search logs using ES|QL.
    
- Machine Learning: Detect anomalies.
    
- Elastic Maps: View geospatial data.
    
- Alerting and reporting tools.
    

Kibana makes it easier to analyze and present log data in a structured way.

---

## ELK vs Splunk (Conceptual Mapping)

- Elasticsearch = Splunk Indexer
    
- Logstash = Splunk Heavy Forwarder
    
- Kibana = Splunk Web UI (Search Head)
    
- Beats/Elastic Agent = Splunk Universal Forwarder
    

This shows how ELK stack components map closely to commercial SIEM solutions, making it easier to transition between tools.

---

## Benefits of Using ELK

- Centralized logging, which helps with compliance and faster investigations.
    
- Flexible ingestion methods.
    
- Rich visualizations and dashboards.
    
- Scalable architecture that can grow with your environment.
    
- Strong ecosystem and community support.
    
- Many commercial SIEM solutions are built on ELK.
    

---

## Key Takeaway

- The ELK Stack is a core skill for SOC analysts.
    
- It is used for SIEM, monitoring, log analysis, and incident response.
    
- Understanding ELK makes it easier to work with or transition into other SIEM platforms.
    
- This stack forms a major part of the SOC lab for this project.


 **Reference**
 https://youtu.be/4AwBhXAW90Q?si=kfoD9D6rQfud7km2