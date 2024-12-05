# Excluding columns from tests

You can omit selected columns from the output comparison by listing them in an `ignore` property array for the relevant output.

```
{   …
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
    ],
    …
}
```