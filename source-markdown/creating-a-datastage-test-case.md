# Creating a DataStage test case

DataStage® test cases are design-time assets that use data files to define the inputs and expected outputs of your DataStage flows.

The basic building blocks of a test case are:
* A test specification
* One or more test data input files
* One or more test data output files

Each DataStage test case is associated with a single DataStage flow. You can create DataStage test cases from the Asset browser or directly from within the DataStage designer canvas.

* Creating a DataStage test case
* Editing a DataStage test case
* [Learn more](#.learn-more)

## Creating a DataStage  test case

You can create DataStage test cases either from the Asset browser or directly from within the DataStage designer canvas.

**From the asset browser**
1. Open an existing project or create a new project.
1. Click **New asset +** > **Create DataStage test case**.

**From the DataStage canvas**

1. Open an existing DataStage flow or create a new flow.
1. Click the **Test cases** icon to open up the test cases side panel.
1. Click **New test case +**

## Defining your test case

1. On the Create test case page specify the name and optional description for the DataStage test case.  If you invoked this action from the asset browser you'll also need to specify a DataStage flow with which to associate this test case. Click **Next**.
1. On the Select stubbed links page select the flow links which will be stubbed in the test.  This determines the ...
    * input links into which test data will be injected, and
    * output link from which data will be compared to the expected output.
1. Click **Next**
1. Specify the names of Parameters or parameter sets to for which your test will supply values. Click **Create**.

