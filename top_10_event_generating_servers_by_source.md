# Top 10 Event-Generating Servers by Data Source

This file contains two KQL queries that extract and visualize the **top 10 servers** (by hostname) generating the most events from two different data sources in Microsoft Sentinel: `SecurityEvent` and `Syslog`.

## Purpose

These queries help identify which servers are generating the highest volume of log events. This can be useful for:

- **Detecting noisy or misconfigured systems**
- **Prioritizing investigation targets**
- **Validating coverage and logging consistency across systems**
- **Identifying potential anomalies or hotspots**

## Query 1: Top 10 Servers from SecurityEvent

```kql
//Windows servers
SecurityEvent
| summarize EventCount = count() by Computer
| top 10 by EventCount

//Linux servers
Syslog
| summarize EventCount = count() by Computer
| top 10 by EventCount