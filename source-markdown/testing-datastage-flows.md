# Testing DataStage flows

## Introduction

Unit tests can be created for individual jobs using the the DataStage user interface.  When DataStage creates a test case it performs the following steps:

1. Interrogate your flow design to identify all source and target stages.
1. Read the metadata definition of each source stage input link and target stage output link (i.e. all data flowing into and out of your flow).  Note that each source stage may be configured with multiple output links, and each target stage may be configured with multiple input links.
1. Create an empty test case data file for each source and target link, with appropriate metadata definitions. 
1. Interrogate your flow's parameters.
1. Create a test case specification which provides references to all of your flow's parameters as well as each newly-created test data file.

## Using DataStage test cases

1. [Creating a DataStage test case](creating-a-datastage-test-case.md)
1. [Editing DataStage test](editing-a-datastage-test.md)
1. [Managing DataStage test data](managing-datastage-test-data.md)
1. [Executing a DataStage test](executing-a-datastage-test-case.md)

## See also

* [DataStage testing concepts](datastage-testing-concepts.md)
* [Building tests with high data volumes](high-volume-tests.md)
* [The DataStage test specification format](test-specification-format.md)
* [Testing DataStage flows](testing-datastage-flows.md)

