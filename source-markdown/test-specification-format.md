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

### Sparse Lookup sources <a href="sparse-l ookup-sources"></a>

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

## Then <a href="then"></a>

The `then` property array associates test data files with your flow's output links. 

For example:
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

Similar to the `Given` property, because some target stages can be configured with multiple input links the test specification's `then` property array uniqely identifies links using a combination of the stage and link names.  The array also contains a `path` property to identify the test data CSV file containing the test data that is to be injected on each incoming link.


### Cluster keys <a href="cluster-keys"></a>

The `cluster` property is used to assist DataStage's resource management when using high volumes of test data.  Setting a **Cluster Key** will prompt DataStage to split the actual output and expected output using multiple, smaller subsets (based on the supplied keys) before the data is compared.  Data is split such that each subset will only contain records that have the same values for all columns that make up the Cluster Key.  In general, Cluster Keys should only be used when necessary and not specified by default. Read more about the using the cluster property in [high volume tests](high-volume-tests.md).

```
{   …
    "then": [
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out",
            "cluster": [
                "Account_Id",
                "Type_Code"
            ]
        }
    ],
    …
}
```

### Row count comparisons <a href="row-count-comparisons"></a>

You can configure a test to only compare outputs' row counts, rather than the content of those rows, by setting the `checkRowCountOnly` property to true.

```
{   …
    "then": [
        {
            "path": "ODBC_orders.csv",
            "stage": "ODBC_order",
            "link": "order_out",
            "checkRowCountOnly": "true"
        }
    ],
    …
}
```

### Excluding columns from tests <a href="excluding-columns-from-tests"></a>

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

## Test specification patterns <a href="test-specification-patterns"></a>

Most DataStage flows can be tested simply by replacing input and output stages. However some flow designs may necessitate a more advanced testing configuration. The sections below outline DataStage test specification patterns that best match these job designs.

* [Testing stages with reject links](testing-stages-with-reject-links.md)
* [Testing Stored Procedure Stages]()
* [Testing Surrogate Key Generator Stages]()
* [Testing Sparse Lookup Stages]()
* [Testing Jobs with current date calculations]()
* [Tests featuring Local and Shared Containers]()

See also:

* [High volume Tests](high-volume-tests.md)


- [Test Specification Types](#test-specification-types)
- [Test specification patterns](#test-specification-patterns)
