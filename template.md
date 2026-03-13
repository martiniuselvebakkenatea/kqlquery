# <Title of Query / Report>

## Description

This Kusto Query Language (KQL) script queries the `<TableName>` table in Microsoft Sentinel to identify `<what the query finds>`.  
It filters for `<key condition(s)>` and summarizes the results by `<grouping field(s)>`.

## Purpose

- **Purpose text here** Purpose details here
- **Purpose text here** Purpose details here
- **Purpose text here** Purpose details here
- **Purpose text here** Purpose details here

## Data Source

- **Log Table:** `<TableName>`
- **Key Fields Used:** `<Field1>`, `<Field2>`, `<Field3>`

## KQL Query

```kusto
<TableName>
| where <FilterCondition>        // <Explain condition>
| summarize <MetricName> = <Aggregation>() by <GroupByField>
| top <N> by <MetricName> desc
