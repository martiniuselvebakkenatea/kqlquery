# Sentinel Heartbeat Monitoring (Case-Insensitive & FQDN-Safe)

## Description

This Kusto Query Language (KQL) script queries the `Heartbeat` table in Microsoft Sentinel to identify whether a predefined list of expected servers has reported a heartbeat within the last 6 hours.
It normalizes hostnames to handle differences in casing and fully qualified domain names (FQDNs), ensuring accurate matching even when computers report as `vm010`, `VM010`, or `vm010.domain.local`.

## Purpose

This query helps validate agent connectivity and server availability by identifying systems that have stopped reporting heartbeats. It is useful for:

* **Detecting servers with offline or unhealthy monitoring agents**
* **Identifying gaps in log ingestion or agent deployment**
* **Verifying monitoring coverage for critical or expected systems**
* **Supporting operational monitoring and alerting in Microsoft Sentinel**

## Data Source

* **Log Table:** `Heartbeat`
* **Key Fields Used:** `Computer`, `TimeGenerated`

## KQL Query

```kusto
let Expected = datatable(Computer:string)
[
  "vm101","vm102","vm103","vm104","vm105","vm106","vm107","vm108"
]
| extend ComputerKey = tolower(tostring(Computer));

let HeartbeatsLastHours =
    Heartbeat
    | where TimeGenerated >= ago(6h)        // Only recent heartbeats
    | extend ComputerKey = tolower(
        extract(@"^([^.]+)", 1, tostring(Computer))
      )                                    // Normalize hostname (strip domain + lowercase)
    | summarize LastHeartbeat = max(TimeGenerated) by ComputerKey;

Expected
| join kind=leftouter HeartbeatsLastHours on ComputerKey
| extend Status = iff(
    isnull(LastHeartbeat),
    "❌ No heartbeat last hour",
    "✅ Heartbeat OK"
)
| project Computer, Status, LastHeartbeat
| order by Status desc, Computer asc
```
