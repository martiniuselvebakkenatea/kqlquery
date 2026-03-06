# Top Accounts by Successful Logons on a Specific Server

## Description

This Kusto Query Language (KQL) script queries the `SecurityEvent` table in Microsoft Sentinel to identify the accounts with the highest number of successful logon events on a specific server.  
It filters for **successful logon events (Event ID 4624)** on computers whose name contains **`SERVER HOSTNAME`** and summarizes the results by **account name**.

## Purpose

*   Identify which accounts are logging on most frequently to a specific server.
*   Support investigation of authentication activity on critical or sensitive hosts.
*   Help detect unusual account usage patterns or potential misuse.
*   Provide visibility into account activity for operational or security reviews.

## Data Source

*   **Log Table:** `SecurityEvent`
*   **Key Fields Used:** `Computer`, `EventID`, `Account`

## KQL Query

```kusto
SecurityEvent
| where Computer contains "SERVER HOSTNAME"   // Filters events for the specified server
| where EventID == 4624                       // Successful logon events
| summarize AccountCount = count() by AccountName = tostring(Account)
| top 10 by AccountCount desc
```

If you want, I can also:

*   Rephrase this for **Sentinel Analytics Rule documentation**
*   Add a **time filter** or **LogonType filtering**
*   Produce a **generic version for multiple servers**
*   Align it with **SOC runbook / detection documentation**

Just tell me how you want to use it.
