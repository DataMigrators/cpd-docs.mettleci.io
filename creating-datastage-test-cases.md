# Creating a DataStage test case

Unit tests can be created for DataStage flows using the DataStage user interface.  When DataStage creates a test case it performs the following steps:

1. Inspect your flow design to identify all source and target stages.
1. Read the metadata definition of each source stage input link and target stage output link (i.e. all data flowing into and out of your flow).  Note that each source stage may be configured with multiple output links, and each target stage may be configured with multiple input links.
1. Create an empty test case data file for each source and target link, with appropriate metadata definitions.
1. Interrogate your flow's parameters.
1. Create a test case specification which provides references to all of your flow's parameters as well as each newly-created test data file.

![screen capture](./images/ds-test-case-generate-csv.png "test screen capture")

## Process

You can create DataStage test cases either as a new asset or directly from within the DataStage designer canvas.

From the CPD Cluster home page open an existing project or create a new project then, on the **Assets** tab, click **New Asset** > **Create reusable DataStage components** > **Test case**.

To edit a test case from the DataStage canvas, go to an existing or new flow and click the **Test cases** icon to open up the test cases panel. Click **New test case**.

## Defining test case properties

1. On the **Create test case** page specify the name and optional description for the DataStage test case.  If you invoked this action when creating a new asset you'll also need to specify a DataStage flow with which to associate this test case.
1. Click **Next**.
1. On the **Select stubbed links** page select the flow links which will be stubbed in the test. This determines the ...
    * input links into which test data will be injected, and
    * output links from which data will be compared to the expected output.
1. Click **Next**
1. Specify the names of parameters or parameter sets for which your test will supply hard-coded values.
1. Click **Create**.

## Creating test data

There are a number of ways you can derive data for your test case.  Start by opening you test case and from the Data tab selecting the test data file you wish to edit.

### Capture test data

Use this method to capture data at runtime from a job invocation.  For more details see [Capturing test data](capturing-test-data.md).

### Import test data

You can import locally-stored CSV data into your IBM Cloud Pak DataStage test data files:

1. Click the **Import** button above the test data table.
1. Upload your file by dragging and dropping your file, or clicking the link to specify a file.
1. Click **Import**.

Note that any existing test data in your selected file will be overwritten.

You can read more about supplying the data and parameters that define your tests in [editing DataStage test cases](editing-datastage-tests.md).
