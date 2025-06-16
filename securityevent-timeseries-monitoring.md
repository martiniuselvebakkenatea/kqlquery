# Security Event Time-Series Monitoring (KQL)

This Kusto Query Language (KQL) script is used in Microsoft Sentinel or Azure Monitor Logs to visualize specific Windows Security Events across selected servers over time.

## Purpose

- Visualize the frequency of specific Windows Security Events across selected servers.
- Identify trends or spikes in event activity over time (e.g., logon failures, privilege use).
- Monitor security-related behavior on critical infrastructure such as domain controllers or file servers.
- Support incident investigation by correlating events across machines and time.
- Establish baselines for normal security event patterns to detect anomalies more effectively.

```kql
SecurityEvent
| where Computer has_any ("server1", "server2", "server3")
| where EventID in (0000, 0000, 0000) // Replace with relevant Event IDs
| summarize EventIDCount = count() by bin(TimeGenerated, 1h), Computer
| render timechart
