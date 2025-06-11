## Top 10 Security Event Types by Server with render pichart

This KQL query filters `SecurityEvent` logs to include only entries from servers with names containing `"SERVERNAME"`. It counts the number of events per `EventID`, converts the ID to a string (required for pie chart visualization), selects the top 10 most frequent event types, and displays the result as a pie chart in Microsoft Sentinel.

### Query

```kql
SecurityEvent
| where Computer contains "SERVERNAME"
| summarize EventCount = count() by EventIDString = tostring(EventID)
| top 10 by EventCount
| render piechart
