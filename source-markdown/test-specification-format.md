# DataStage test specification format

- [Structure](#structure) overview
- [Given](#given) these inputs
  - [Sparse Lookup sources](#sparse-lookup-sources)
- [When](#when) these conditions are met
- [Then](#then) expect these outputs

## Structure <a id="structure"></a>

A DataStage test case specification (often abbreviated ‘Spec') is a JSON-formatted file which uses a grammar modelled loosely on the [Gherkin syntax](https://cucumber.io/docs/gherkin/) used by the Cucumber testing tool. The overall structure follows the common Gherkin pattern …

```text
{
    "given": [
        { This test data on input link 1 },
        { This test data on input link 2 }
    ],
    "when": {
        I execute the test case with these options and parameter values
    },
    "then": [
        { Expect this data to appear on output link 1 },
        { Expect this data to appear on output link 2 }
    ]
}
```

***Note:*** The user interface may order the JSON objects alphabetically (`given` > `then` > `when`) but this has no effect on the functionality of the test.

## Given <a id="given"></a>

The `given` property array associates test data files with your flow's input , thereby defining the test values you wish to inject into your flow's inputs at runtime.

For example:

```json
{
    "given": [
        {
            "path": "fileCustomers.csv",
            "stage": "sfCustomers",
            "link": "Customers" 
        },
        {
            "path": "fileOrders.csv",
            "stage": "sfOrders",
            "link": "Orders"
        }
    ],
}
```

Some source stages can be configured with multiple output links so each input in your test specification's `given` property array is uniquely identified using a combination of the stage and link names to eliminate ambiguity.  The array also contains a `path` property to identify the test data CSV file containing the test data that is to be injected on each incoming link.

Note that not every stage in a job must be provided with test data.  You can easily craft a test specification which [uses test data for only a subset of flow stages](selective-stubbing.md).

### Sparse Lookup sources <a id="sparse-lookup-sources"></a>

When an input source is used with a Sparse Lookup stage then rather than using the stage property to specify the input you will use the `sparseLookup` property.

For example:

```json
{
    "given": [
        {
            "path": "fileCustomers.csv",
            "stage": "sfCustomers",
            "link": "Customers" 
        },
        {
            "sparseLookup": "SparseLookup",
            "path": "Database-Reference.csv",
            "key": [
                "KEY_COLUMN_1",
                "KEY_COLUMN_2"
            ]
        }
    ],
}
```

The `sparseLookup` property identifies a JSON object which specifies …

- the value defining the name of the sparse lookup reference stage,
- a path to the relevant CSV test data file, and
- a list of key columns to be used for the sparse lookup.

## When <a id="when"></a>

The `when` property array specifies which job will be executed during testing as well as any parameters (including job macros) that affect the data produced by the job.

For example, this specification will

Substitute hardcoded values for the `DSJobStartDate` and `DSJobStartTime` macros and the `paramStartKey` parameter:

```json
{
    "when": {
        "data_intg_flow_ref": "3023970f-ba2dfb02bd3a",  
        "parameters": {
            "DSJobStartDate": "2012-01-15"
            "DSJobStartTime": "11:05:01"
            "paramStartKey": "100"
        }
    },
}
```

One application of the `parameters` property is to supply values to make flows that rely on system date and time information produce a deterministic output by [hard coding those values when testing](testing-flows-using-datetime-references.md).

**Note** that the `data_intg_flow_ref` property is an internally-generated DataStage reference to the flow with which this test is associated and should not be changed.

## Then <a id="then"></a>

The `then` property array associates test data files with your flow's output links.

```json
{
    "then": [
        {
            "path": "ODBC_customers.csv",
            "stage": "ODBC_customer",
            "link": "customer_out"
        },
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out"
        }
    ],
}
```

Similar to the `given` property, because some target stages can be configured with multiple input links the test specification's `then` property array uniquely identifies links using a combination of the stage and link names. The array also contains a `path` property to identify the test data CSV file containing the expected test output that will be compared to the actual data appearing on each outgoing link.

Other properties which extend the capabilities of your test case can be included in the `then` property array:

- **The `ClusterKey` property**: [Improve performance of test cases when using data volumes](high-volume-tests.md)
- **The `checkRowCountOnly` property**: [Configure your tests to only count the number of rows](row-count-comparisons.md)
- **The `ignore` property**: [Exclude specific columns from test comparisons](excluding-columns-from-tests.md)
