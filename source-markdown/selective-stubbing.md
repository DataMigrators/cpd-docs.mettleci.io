# Selective stubbing

The process of 'stubbing' involves using a fabricated version of an external data source (a 'stub') that returns specific, deterministic values or behaviors. Stubs can be used to test code that relies on external data sources that are not available at a given time or in a given environment, or which are non-deterministic.

When you [specify a DataStage unit test](creating-datastage-test-cases.md) you are defining stub data and telling DataStage which link(s) to stub with that data. There may be some instances where you wish to define a test case that only injects test data into ***some*** of your flow's input links, and allow other source stages to operate normally during a test execution. These input links which are not stubbed are not referenced in your test specification and will connect to their configured data source and retrieve data at runtime.

To define a link which should not be stubbed simply omit the link from the `given` section of your [test specification](test-specification-format.md).

For example ...

```json
{
    "given": [
        {
            "path": "filePurchasesIn.csv",
            "stage": "dsEX_Purchase",
            "link": "inPurchase" 
        }
    ],
    "then": [
        {
            "path": "filePurchasesOut.csv",
            "stage": "dsTR_Purchase",
            "link": "outPurchase"
        }
    ],
    "when": {
        "data_intg_flow_ref": "blah-blah-blah",  
        "parameters": {
        }
    }
}
```

&nbsp;
![screen capture](./images/ds-test-case-selective-stubbing.png "test screen capture")

***Note***: When defining test cases that use selective stubbing you should to exercise caution when deploying those test cases to downstream test environments.  Those environments will need to be configured to permit stages which have not been stubbed to retrieve data from their configured data sources.
