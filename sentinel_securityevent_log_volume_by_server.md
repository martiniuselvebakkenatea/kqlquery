# SecurityEvent Log Volume by Server

## Description

This Kusto Query Language (KQL) script queries the `SecurityEvent` table in Microsoft Sentinel to identify **which servers generate the most Windows security logs**.
It aggregates the total number of events per server and orders the results by **log volume in descending order**.

## Purpose

* **Log source visibility:** Identify which servers are generating the highest volume of security events.
* **Baseline monitoring:** Establish a normal log generation baseline for Windows hosts.
* **Troubleshooting ingestion:** Detect systems that may be generating unusually high or low event volumes.
* **Infrastructure insight:** Quickly see which systems are most active from a security logging perspective.

## Data Source

* **Log Table:** `SecurityEvent`
* **Key Fields Used:** `Computer`, `TimeGenerated`

## KQL Query

```kusto
SecurityEvent
| summarize LogCount = count() by ServerName = tostring(Computer)
| order by LogCount desc
```

### Query Explanation

| Step                                 | Explanation                                                       |
| ------------------------------------ | ----------------------------------------------------------------- |
| `SecurityEvent`                      | Queries the Windows Security Event log data collected in Sentinel |
| `summarize count()`                  | Counts the number of security events                              |
| `by ServerName = tostring(Computer)` | Groups results by the server hostname                             |
| `order by LogCount desc`             | Sorts results to show the servers generating the most logs first  |
