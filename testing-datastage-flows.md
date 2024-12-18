# Getting Started With Testing DataStage Flows

DataStage® test cases are design-time assets that use data files to define the inputs and expected outputs of your DataStage flows.

The basic building blocks of a test case are:

- A test specification
- One or more test data input files
- One or more test data output files

Each DataStage test case is associated with a single DataStage flow. You can create DataStage test cases as a new asset or directly from within the DataStage designer canvas.  A DataStage test case is executed by a job in a manner similar to its associated DataStage flow. During execution the input data files are injected into your flow's incoming links and the data appearing on the output links are compared to the output files containing the expected outputs. Any differences in the two will cause the test to fail and the differences to be reported alongside the test case's job log.  

## Using DataStage test cases

1. [Configuring test data storage](configuring-test-data-storage.md)
1. [Creating DataStage test cases](creating-datastage-test-cases.md)
1. [Editing DataStage tests](editing-datastage-tests.md)
1. [Executing DataStage tests](executing-datastage-test-cases.md)
1. [Verifying DataStage test results](verifying-test-results.md)
1. [Migrating test cases from older versions of DataStage](migrating-datastage-tests-from-older-versions.md)
