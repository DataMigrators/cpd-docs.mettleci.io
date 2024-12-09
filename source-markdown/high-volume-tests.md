# High Volume DataStage Tests

During the execution of a DataStage test case the data produced by a job (on one or more output links) is compared against expected test data to identify and report on any differences.  When testing with large volumes of data, the comparison process may consume too much memory and cause your test job to abort with a fatal error.  The simplest approach to resolving this issue is to reduce your test data volume to the smallest number of records necessary to exercise each code path through your flow.  Doing so will ensure that your test cases execute quickly and can be easily understood and maintained.

In the event that test data available to you cannot easily be reduced, the memory required by the data comparison process can be reduced by specifying a **Cluster Key** in the test specification.

## Using Cluster Keys in DataStage Tests

Defining a Cluster Key will cause DataStage to split the actual data output and expected data into multiple, smaller subsets before the data is compared.  Data is split such that each subset will only contain records that have the same values for all columns that make up the Cluster Key - a process somewhat analogous to DataStage partitioning.  The data are then sorted and a comparison of actual and expected data is performed using multiple, smaller operations which require less memory and are performed sequentially.  

**Test result behavior**: Due to the iterative nature of comparisons using a Cluster Key, each record which has differences in the Cluster Key columns will be reported as 1 added record and 1 removed record rather than shown as a single record with a change indicator.

A good Cluster Key is one that results in data subsets which strike a balance between the following factors:

* Each subset should fit in memory during comparison. Test execution will abort when memory thresholds are breached.
* Are as large as possible given the memory constraint. Lots of tiny subsets will degrade comparison performance.

Selecting an appropriate Cluster Key might require several iterations to find a column (or combination of columns) which not only prevents Job aborts but also keeps run times acceptable.  Unless you are comparing unusually wide records, a good starting point is to aim for each subset of data to contain no more than 1,000 records and adjust the Cluster Key if memory thresholds continue to be breached.

> If you have used Interception to capture some input and / or expected test data for a DataStage Test and subsequently decide you want to apply a Cluster Key, you don’t have to re-run Interception. This is also the case if you’ve manually created any test data files. The Cluster Key is used at run time and therefore doesn’t require any additional data preparation by the user.

## Example

Consider the situation where a DataStage Test has to compare several million financial transaction records with the following schema on the 'order_out' link of stage 'ODBC_order':

| Column name      | SQL type  | Length | Scale | Nullable |
|------------------|-----------|--------|-------|----------|
| Transaction_Date | Timestamp |        |       | No       |
| Account_Id       | VarChar   |     20 |       | No       |
| Type_Code        | VarChar   |      5 |       | No       |
| Description      | VarChar   |        |       | Yes      |
| Amount           | Decimal   |     18 |     2 | No       |

The test specification can be updated with a Cluster Key to enable iterative comparison of actual and expected test data.  In this example, `Account_Id` and `Type_Code` are defined as the compound Cluster Key:

```json
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

**Note**: Cluster Keys are specified on a per-link basis. DataStage flows with multiple output links can use any combination of clustered and non-clustered comparisons within a single test specification.

### Caveats

Cluster keys should be chosen to break Actual and Expected data into clusters which are small enough to fit in memory.

Note that if a Unit Test detects a value difference in a column which is a cluster key column, then the Unit Test difference report (which would normally describe the difference as a ***‘modified’*** row when not using a cluster key) will now describe the difference as distinct ***‘added’*** and ***‘removed’*** entries.  

As useful as Cluster Keys are, it’s poor practice to simply apply them to every DataStage test that has to process high data volumes. You will almost certainly find combinations of flows and data volumes in your project where no Cluster Key will reduce the memory demands of a DataStage test enough to avoid Job aborts **(See Unit Test throws OutOfMemoryError exception)**. In these situations you can manage your test data volumes by …

* carefully selecting a subset of records from your data sources,
* using the DataStage's data fabrication features, or
* both of these approaches in combination.
