# Creating a DataStage test case

Unit tests can be created for individual jobs using the the DataStage user interface.  When DataStage creates a test case it performs the following steps:

1. Interrogate your flow design to identify all source and target stages.
1. Read the metadata definition of each source stage input link and target stage output link (i.e. all data flowing into and out of your flow).  Note that each source stage may be configured with multiple output links, and each target stage may be configured with multiple input links.
1. Create an empty test case data file for each source and target link, with appropriate metadata definitions. 
1. Interrogate your flow's parameters.
1. Create a test case specification which provides references to all of your flow's parameters as well as each newly-created test data file.

&nbsp;
![screen capture](./images/ds-test-case-generate-csv.png "test screen capture")

## Creating a DataStage test case

You can create DataStage test cases either from the Asset browser or directly from within the DataStage designer canvas.

From the asset browser open an existing project or create a new project then click **New asset +** > **Create DataStage test case**.

From the DataStage canvas open an existing DataStage flow or create a new flow then click the **Test cases** icon to open up the test cases side panel and click **New test case +**.

## Defining test case properties

1. On the **Create test case** page specify the name and optional description for the DataStage test case.  If you invoked this action from the asset browser you'll also need to specify a DataStage flow with which to associate this test case.
1. Click **Next**.
1. On the **Select stubbed links** page select the flow links which will be stubbed in the test. This determines the ...
    * input links into which test data will be injected, and
    * output links from which data will be compared to the expected output.
1. Click **Next**
1. Specify the names of parameters or parameter sets for which your test will supply hard-coded values.
1. Click **Create**.

