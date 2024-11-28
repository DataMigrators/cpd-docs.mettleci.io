# Creating a DataStage unit test case

DataStage® unit test cases are design-time assets that define a set of input data for a DataStage flow and, for those input data, a set of expected output data.

You can create an DataStage unit test case and specify, fabricate, or capture input and output data.

The basic building blocks of a flow are:
Data sources that read data
Stages that transform the data
Data targets that write data
Links that connect the sources, stages, and targets


You can create automated unit tests for DataStage® flows.  A unit test comprises a unit test specification and a collection of unit test data files. 

A DataStage unit tests are .... 

You can manage all your DataStage components, including DataStage unit test cases, from the Assets tab.

1. Open an existing project or create a project.
1. Click New asset + > Create reusable DataStage components.
1. Select the DataStage component that you want to create type, then click Next.
1. Specify details such as name and description for the DataStage component, then click Create.
1. Provide any further details that your DataStage component requires. When you are finished, click Save.

The following DataStage components are available:

* Complex Flat File schema
* Data definitions
* Schema library
* Standardization rule
* Subflow

**Parent topic:** [Unit Testing DataStage flows](unit-testing-datastage-flows.md)