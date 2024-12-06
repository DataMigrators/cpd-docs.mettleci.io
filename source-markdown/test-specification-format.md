# DataStage test specification format

- [Structure](#structure)
- [Given](#given)
-  - [Sparse Lookup sources](#sparse-lookup-sources)
- [When](#when)
- [Then](#then)
   - [Cluster keys](#cluster-keys)
   - [Row count comparisons](#row-count-comparisons)
   - [Excluding columns from tests](#excluding-columns-from-tests)
- [Test specification patterns](#test-specification-patterns)

## Structure <a href="structure"></a>

A DataStage test case specification (often abbreviated ‘Spec') is a JSON-formatted file which uses a grammar modelled loosely on the [Gherkin syntax](https://cucumber.io/docs/gherkin/) used by the Cucumber testing tool. The overall structure follows the common Gherkin pattern …

```
{
    "given": [
        { # Inject this test data file into stageA.linkA },
        { # Inject this test data file into stageB.linkB },
        { # etc. }
    ],
    "when": {
        # Execute the test case with these options and parameter values
    },
    "then": [
        { # Expect flow output on stageX.linkX to look like this test data file },
        { # Expect flow output on stageY.linkY to look like this test data file },
        { # etc. }
    ],
}
```

***Note:*** The user interface may order the JSON objects alphabetically (`given` > `then` > `when`) but this has no effect on the functionality of the test.

## Given <a href="given"></a>

The `given` property array associates test data files with your flow's input , thereby defining the test values you wish to inject into your flow's inputs at runtime.

For example:
```
{   …
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
    …
}
```

Some source stages can be configured with multiple output links so each input in your test specification's `given` property array is uniqely identified using a combination of the stage and link names to eliminate ambiguity.  The array also contains a `path` property to identify the test data CSV file containing the test data that is to be injected on each incoming link.

Note that not every stage in a job must be provided with test data.  You can easily craft a test specification which [uses test data for only a subset of flow stages](selective-stubbing.md).

### Sparse Lookup sources <a href="sparse-lookup-sources"></a>

When an input source is used with a Sparse Lookup stage then rather than using the stage property to specify the input you will use the `sparseLookup` property.

For example:
```
{   …
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
    …
}
```

The `sparseLookup` property identifies a JSON object which specifies …

* the value defining the name of the sparse lookup reference stage,
* a path to the relevant CSV test data file, and
* a list of key columns to be used for the sparse lookup.

## When <a href="when"></a>

The `when` property array specifies which job will be executed during testing as well as any parameters (including job macros) that affect the data produced by the job.

For example, this specification will

Substitute hardcoded values for the `DSJobStartDate` and `DSJobStartTime` macros and the `paramStartKey` parameter:

```
{   …
    "when": {
        # An internally-generated reference to the flow with which this test is associated
        "data_intg_flow_ref": "3023970f-ba2dfb02bd3a",  
        "parameters": {
            "DSJobStartDate": "2012-01-15"
            "DSJobStartTime": "11:05:01"
            "paramStartKey": "100"
        }
    },
    …
}
```

One application of the `parameters` property is to supply values to make flows that rely on system date and time information produce a deterministic output by [hard coding those values when testing](testing-flows-using-datetime-references.md).

## Then <a href="then"></a>

The `then` property array associates test data files with your flow's output links. 

```
{   …
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
    …
}
```

Similar to the `given` property, because some target stages can be configured with multiple input links the test specification's `then` property array uniqely identifies links using a combination of the stage and link names.  The array also contains a `path` property to identify the test data CSV file containing the test data that is to be injected on each incoming link.

Other properties which extend the capabilities of your test case can be included in the `then` property array:

**The `ClusterKey` property**: [Improve performance of test cases when using data volumes](high-volume-tests.md)

**The `checkRowCountOnly` property**: [Configure your tests to only count the number of rows](row-count-comparisons.md)

**The `ignore` property**: [Exclude specific columns from test comparisons](excluding-columns-from-tests.md)
