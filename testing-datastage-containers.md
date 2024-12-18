# Test Cases with local and shared containers

Page numberingPage numberingScroll page detailsScroll page detailsScroll ViewportScroll Viewport
The structure of a MettleCI Unit Test Specification (“Spec”) is modelled loosely on the Gherkin syntax of a testing tool called Cucumber.  It associates Unit Test data, which are stored in CSV files identified by the path property, with each of your Job’s input and output links, which are identified by the stage and link properties of your Spec. The Given clause defines the Unit Test data for your Job’s input links and the Then clause for the output links.  

Here's a simple example:

![representation of a DataStage job showing CSV files being injected into source stages and an output stage referring to a CSV file which does not exist](./images/ds-test-case-simple2.png "test baseline screen capture")

Containers (both local and shared) complicate this situation as stage names in DataStage are only unique within a given flow or container.  

Consider writing a test specification for the following Job `ProcessAccounts` which includes a shared container stage `ContainerC1` which is a reference to the shared container `scWriteAccounts`:

Flow:
![representation of a DataStage flow using a shared container](./images/ds-test-case-container2.png "test cases using containers")

Container:
![representation of a DataStage shared container](./images/ds-test-case-container5.png "test cases using containers") |

You might be tempted to create a spec like this ...

```json
{
    "given": [
        {
            "path": "given.csv",
            "stage": "sfInput",
            "link": "in" 
        }
    ],
    "when": {},
    "then": [
        {
            "path": "expected.csv",
            "stage": "sfOutput",
            "link": "out"
        }
    ]
}
```

The resulting Unit Test Spec is ambiguous because the MettleCI Unit Test Harness will not be able to uniquely identify which Unit Test Data file is associated with each sqAccounts stage.  To avoid these sort of issues, the stage properties within Unit Test Specs expect fully qualified stage names.  A fully qualified stage name is prefixed with any relevant parent Container names using the format `<container name>.<stage name>`.

Here’s an example of a fully qualified stage name:

```json
{
    "given": [
        {
            "path": "given.csv",
            "stage": "sfInput",
            "link": "in" 
        }
    ],
    "when": {},
    "then": [
        {
            "path": "expected.csv",
            "stage": "subflow_1.sfOutput",
            "link": "out"
        }
    ]
}
```
