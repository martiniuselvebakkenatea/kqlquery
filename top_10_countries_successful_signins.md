# Top 10 Countries with Successful Sign-Ins in Microsoft Sentinel

## Description

This Kusto Query Language (KQL) script queries the `SigninLogs` table in Microsoft Sentinel to identify the top 10 countries from which successful sign-ins have occurred. It filters for successful sign-in attempts (`ResultType == 0`) and then summarizes the results by country.

## Purpose

- **Threat Hunting:** Spot unusual successful sign-ins from unexpected countries.
- **User Behavior Analytics:** Understand where your user base is logging in from.
- **Security Baselines:** Build a baseline of expected geographic locations for sign-ins.
- **Compliance and Auditing:** Generate reports for compliance audits related to geographic access.
- **Access Policy Tuning:** Use the data to adjust Conditional Access policies based on location.
  
## KQL Query

```kusto
SigninLogs
| where ResultType == 0  // 0 = Success
| summarize SuccessfulSignins = count() by Location
| top 10 by SuccessfulSignins desc
