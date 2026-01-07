# <Title of Query / Report>

## Description

This Kusto Query Language (KQL) script queries the `<TableName>` table in Microsoft Sentinel to identify `<what the query finds>`.  
It filters for `<key condition(s)>` and summarizes the results by `<grouping field(s)>`.

## Purpose

- **Point 1:** Write some text here
- **Point 2:** Write some text here
- **Point 3:** Write some text here
- **Point 4:** Write some text here

## Data Source

- **Log Table:** `<TableName>`
- **Key Fields Used:** `<Field1>`, `<Field2>`, `<Field3>`

## KQL Query

```kusto
<TableName>
| where <FilterCondition>        // <Explain condition>
| summarize <MetricName> = <Aggregation>() by <GroupByField>
| top <N> by <MetricName> desc
