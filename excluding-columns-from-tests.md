# Excluding columns from tests

You can omit selected columns from the test case output comparison by adding the columns to be ignored to an `ignore` array in the relevant specification's `then` property.

```json
{
    "then": [
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out",
            "ignore": [
                "Creation_date",
                "Last_updated"
            ]
        }
    ]
}
```

**Note**: Ignoring columns will prevent columns containing non-deterministic from affecting test results but will also omit those columns from test comparisons, so unexpected output in those columns, or changes in the output of those columns, will not be detected by your test case.  This reduces your test coverage and should be avoided if possible. A common technique to avoid non-deterministic outputs is to ensure all inputs data sources and flow parameters are stubbed by your test specification.
