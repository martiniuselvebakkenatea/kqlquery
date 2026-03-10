# MTTR by Severity (Closed Incidents) — Microsoft Sentinel

## Description

This Kusto Query Language (KQL) script queries the `<TableName>` table in Microsoft Sentinel to calculate **Mean Time To Resolve (MTTR)** for security incidents. It filters for incidents that are **Closed** and ensures a **ClosedTime** exists, then calculates the time between **CreatedTime** and **ClosedTime** (in minutes). Finally, it summarizes the results by **Severity**, returning both the average resolution time and the number of incidents per severity, sorted ascending by severity. 

## Purpose

*   **Measure SOC efficiency:** Provide an at‑a‑glance MTTR metric per severity to understand how quickly incidents are being resolved once created.
*   **Identify bottlenecks:** Spot severities that consistently take longer to resolve, which may indicate workflow, ownership, or tooling gaps. 
*   **Support reporting & SLA discussions:** Produce a repeatable metric that can feed operational reporting and customer/stakeholder conversations about incident handling timelines. (This aligns with how security incidents and service levels are described in internal service documentation.) 
*   **Trend tracking over time (when paired with a time filter):** Use as a baseline query that can be extended with a timeframe (e.g., last 30 days / last month) to track MTTR improvements or regressions.

## Data Source

*   **Log Table:** `<TableName>` = `SecurityIncident` 
*   **Key Fields Used:** `Status`, `ClosedTime`, `CreatedTime`, `Severity` 

### Output Fields Produced

*   `MTTR_Minutes` = average minutes from incident creation to incident closure (per severity)
*   `Incidents` = number of closed incidents included in the calculation (per severity)

## KQL Query

```kusto
SecurityIncident
| where Status == "Closed"                          // Only include incidents that are closed
| where isnotempty(ClosedTime)                      // Ensure ClosedTime exists so resolution time is calculable
| extend TimeToResolve = datetime_diff("minute", ClosedTime, CreatedTime) 
                                                    // Minutes between CreatedTime and ClosedTime
| summarize 
    MTTR_Minutes = avg(TimeToResolve),              // Mean Time To Resolve (minutes)
    Incidents = count()                             // Number of closed incidents in the dataset
  by Severity                                       // Group results by incident severity
| order by Severity asc                             // Sort output by severity
```

***

## (Optional) Practical improvements you may want (common add-ons)

These are **suggestions** (not required by your template), but they’re often useful in reporting:

1.  **Exclude Informational** (you’ve used this before in Teams):

```kusto
| where Severity != "Informational"
```
