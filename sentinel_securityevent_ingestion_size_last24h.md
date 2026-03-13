# SecurityEvent Ingestion Size (Last 24 Hours)

## Description

This Kusto Query Language (KQL) script queries the `SecurityEvent` table in Microsoft Sentinel to calculate **the total volume of ingested SecurityEvent logs over the last 24 hours**.
It filters for events generated within the **previous 24 hours** and summarizes the results by **summing the `_BilledSize` field**, which represents the billable ingestion size of each record.

The total size is then converted from **bytes to megabytes (MB)** for easier interpretation.

## Purpose

* **Ingestion monitoring:** Determine how much SecurityEvent data is ingested in the last 24 hours.
* **Cost awareness:** Estimate ingestion-related costs tied to Windows security event logs.
* **Log volume analysis:** Identify spikes or anomalies in SecurityEvent data volume.
* **Capacity planning:** Understand log growth trends for storage and SIEM usage.

## Data Source

* **Log Table:** `SecurityEvent`
* **Key Fields Used:** `TimeGenerated`, `_BilledSize`

## KQL Query

```kusto
SecurityEvent
| where TimeGenerated > ago(24h)          // Filter logs generated within the last 24 hours
| summarize TotalSizeBytes = sum(_BilledSize)
| extend TotalSizeMB = round(TotalSizeBytes / 1024 / 1024, 2)
| project TotalSizeMB
```

### Query Explanation

| Step                             | Explanation                                                   |
| -------------------------------- | ------------------------------------------------------------- |
| `where TimeGenerated > ago(24h)` | Filters SecurityEvent logs generated within the last 24 hours |
| `sum(_BilledSize)`               | Calculates total ingestion size in bytes                      |
| `extend TotalSizeMB`             | Converts bytes to megabytes                                   |
| `project`                        | Returns only the final MB value for reporting                 |
