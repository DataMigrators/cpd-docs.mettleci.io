# Row count comparisons

You can configure a DataStage test case to only compare outputs' row counts, rather than the content of those rows, by setting the `checkRowCountOnly` property to true.

```
{   …
    "then": [
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out",
            "checkRowCountOnly": true
        }
    ],
    …
}
```

**Note** that the 'checkRowCountOnly' property takes a boolean value which does not use quotes.