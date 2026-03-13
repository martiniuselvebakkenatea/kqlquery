# Sentinel Table Last Log Overview

## Description

This Kusto Query Language (KQL) script queries **all tables in the Microsoft Sentinel / Log Analytics workspace** using the `search` operator to identify **which tables contain data and the most recent log timestamp for each table**.
It summarizes the results by **table name** and calculates both the **total number of records** and the **latest `TimeGenerated` value**.

## Purpose

* **Workspace visibility:** Quickly see which Sentinel tables are actively receiving logs.
* **Connector health monitoring:** Identify data sources that may have stopped ingesting data.
* **Inventory of active tables:** Discover which tables currently contain data in the workspace.
* **Operational troubleshooting:** Assist analysts in diagnosing ingestion or connector issues.

## Data Source

* **Log Table:** `All tables in the Log Analytics workspace`
* **Key Fields Used:** `$table`, `TimeGenerated`

## KQL Query

```kusto
search *
| summarize Count=count(), LastLog=max(TimeGenerated) by $table
| sort by LastLog desc
```

### Query Explanation

| Step                      | Explanation                                            |
| ------------------------- | ------------------------------------------------------ |
| `search *`                | Searches across **all tables** in the workspace        |
| `summarize Count=count()` | Counts total records per table                         |
| `max(TimeGenerated)`      | Finds the **most recent log timestamp** for each table |
| `by $table`               | Groups results by table name                           |
| `sort by LastLog desc`    | Shows the most recently updated tables first           |
