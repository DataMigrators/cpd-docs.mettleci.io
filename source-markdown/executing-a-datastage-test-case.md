# Executing a DataStage test case

When DataStage executes your test case it will dynamically replace your flow's input stages at runtime with components which inject data from the relevant CSV data files into your job on the links specified in [your test specification](test-specification-format.md).  Any source data repositories (files, databases, etc.) included in your test specification will not be connected to or read during a test case execution.

Similarly, your flow's output stage(s) will be replaced at runtime 

![screen capture](./images/ds-test-case-execution.png "test screen capture")


## Step by step

You can execute DataStage test cases either from the Asset browser or directly from within the DataStage designer canvas.

**From the asset browser**
1. Open an existing project project.
1. Under **Asset types** tab select **DataStage components** > **Test cases**.
1. Click the name of the test case you wish to invoke.

**From the DataStage canvas**
1. Open an existing DataStage flow.
1. Click the **Test cases** icon to open up the test cases side panel.
1. Click the run icon alongside the name of the test case you wish to invoke.

This will bring you to the test case editor which suports the following functions:

* [Inspecting your test case JSON specification](test-specification-format.md)
* [Editing input and output metadata](#editing-metadata)



# Editing metadata <a href="editing-metadata"></a>




