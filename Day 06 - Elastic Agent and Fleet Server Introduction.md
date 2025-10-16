
## Overview

Elastic Agent is a **unified agent** that simplifies monitoring and telemetry collection by handling logs, metrics, and other data sources through a single installation. It uses policies to instruct endpoints on what data to collect and where to forward it (Elasticsearch or Logstash).

This replaces the need to install multiple Beats on a single machine, making it more efficient to manage larger environments.

---

## Elastic Agent

- Provides unified monitoring for logs, metrics, and various telemetry sources.
    
- Uses **policies** to control what data is collected and how it’s forwarded.
    
- Integrates directly with Elasticsearch or Logstash.
    
- Allows centralized configuration and lifecycle management (when used with Fleet).
    

### Installation Methods

- **Fleet-managed**:
    
    - All configuration and agent lifecycle are managed through the Fleet UI.
        
    - Makes it easy to deploy new integrations and update configurations centrally.
        
- **Standalone**:
    
    - Configuration is applied directly to each agent manually.
        
    - Useful for smaller setups or when Fleet is not deployed.
        

---

## Beats vs Elastic Agent

- **Beats**:
    
    - Multiple Beats (e.g., Filebeat, Winlogbeat, Metricbeat) may be required on one host.
        
    - Each Beat is configured individually.
        
- **Elastic Agent**:
    
    - Single installation that covers multiple data types.
        
    - Easier to manage at scale.
        
    - Reduces maintenance overhead.
        

---

## Fleet Server

- A **Fleet Server** is a key component that connects Elastic Agents to a Fleet for centralized management.
    
- It allows managing large numbers of agents from one place instead of configuring each manually.
    

### Benefits

- Easy policy updates and addition of new integrations for data ingestion.
    
- Forward data to Elasticsearch or Logstash.
    
- Push agent version updates.
    
- Unenroll specific agents easily.
    

### Alternatives (if Fleet Server isn’t used)

- Manual configuration on each endpoint.
    
- Using Group Policies for distribution and management.
    

---

## Scenario Example

Managing 100 Windows endpoints:

- **Manual configuration**: Time-consuming, prone to error.
    
- **Group Policy**: More automated but still limited.
    
- **Fleet Server**: Centralized, scalable, and efficient for updating and monitoring multiple agents at once.
    

---

## Key Takeaway

- Elastic Agent simplifies log collection and monitoring compared to multiple Beats.
    
- Fleet Server provides centralized control for managing many endpoints efficiently.
    
- For large environments, Fleet Server is the recommended approach to ensure scalability and easy updates.
    
- Standalone installation can work for smaller or isolated deployments but is harder to maintain over time.

**Reference**
https://youtu.be/0WklP6ZsP1g?si=bi4v5y0OQv0NDGYW