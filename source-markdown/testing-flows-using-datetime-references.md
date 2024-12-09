# Testing flows using date/time references

Repeatable DataStage tests require that the flow being tested produces deterministic data based on a set of predefined inputs and parameters.  Calculations based on the current date or time are common in DataStage flow designs but will cause the output produced by those flows to change depending on the date and time when its job is executed.  This page outlines practices aimed at ensuring jobs using current date calculations are able to be validly tested.

## Transformer Stages using system time and date functions

Transformer stages with derivations using the `CurrentDate()`, `CurrentTime()` or `CurrentTimestamp()` functions need to be considered carefully whn designing your tests. Unless your calculation requires a specific date and/or time then calls to the standard `CurrentDate()`, `CurrentTime()` and `CurrentTimestamp()` functions could potentially be substituted with calls to the `DSJobStartDate`, `DSJobStartTime` and `DSJobStartTimestamp` macros. This enables you to substitute them with a specific value during testing.  Add `DSJobStartDate`, `DSJobStartTime`, and `DSJobStartTimestamp` (as required) to the `parameters` clause of the test Specification's `when` node and set the appropriate date and time values used during Unit Testing.

For example, imagine two Transformer stage variables using the following expressions:

```text
BeforeToday =
If inPurchases.Purchase_Date < DSJobStartDate Then "Yes" else "No"

EarlierToday = 
If inPurchases.Purchase_Date = DSJobStartDate and 
   inPurchases.Purchase_Time < DSJobStartTime Then "Yes" else "No"
```

The `when` section of your test specification would look like this:

```json
{   …
    "when": {
        "data_intg_flow_ref": "blah-blah-blah",  
        "parameters": {
            "DSJobStartDate": "2012-01-15",
            "DSJobStartTime": "11:05:01"
        }
    }
}
```

**Note:** Be careful when setting `DSJobStartTimestamp` in conjunction with either `DSJobStartDate` or `DSJobStartTime` as the DataStage test case capability does not attempt to enforce logical consistency between these parameters.
