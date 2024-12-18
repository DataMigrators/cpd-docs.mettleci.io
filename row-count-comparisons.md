# Row count comparisons

You can configure a DataStage test case to only compare outputs' row counts, rather than the content of those rows, by setting the `checkRowCountOnly` property to true.

```json
{
    "then": [
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out",
            "checkRowCountOnly": true
        }
    ],
}
```

**Note** that the `checkRowCountOnly` property takes a boolean value which does not use quotes.

The [test case report](verifying-test-results.md) containing a single cell comparing the expected and actual output row count (of the form 'Expected > Actual'.)

![rowcount comparison](./images/ds-test-rowcount-diff.png "rowcount diff")
