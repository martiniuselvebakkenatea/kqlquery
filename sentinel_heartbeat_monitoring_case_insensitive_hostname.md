# Sentinel Heartbeat Monitoring (Case-Insensitive & FQDN-Safe)

## Description

This Kusto Query Language (KQL) script queries the `Heartbeat` table in Microsoft Sentinel to identify whether a predefined list of expected servers has reported a heartbeat within the last 6 hours.
It normalizes hostnames to handle differences in casing and fully qualified domain names (FQDNs), ensuring accurate matching even when computers report as `vm010`, `VM010`, or `vm010.domain.local`.

## Purpose

* **Point 1:** Detect missing heartbeats for critical or expected servers
* **Point 2:** Handle inconsistent hostname casing across data sources
* **Point 3:** Support environments where agents report FQDNs instead of short hostnames
* **Point 4:** Provide a simple operational health check for server monitoring in Sentinel

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
