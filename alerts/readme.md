Query to Detect No Transaction Flow
This query checks for the absence of transactions within the last 5 minutes.

```
index=transactions_index sourcetype=transaction_logs
| stats count as transaction_count
| where transaction_count == 0
```

Additional Option: Monitoring Drop in Transaction Volume
If you expect a certain transaction rate and want to alert when the volume drops significantly (e.g., by 50% compared to the previous hour), use this query:

```
index=transactions_index sourcetype=transaction_logs
| timechart span=5m count as transaction_count
| eventstats avg(transaction_count) as avg_count
| where transaction_count < (0.5 * avg_count)
```
