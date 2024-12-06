# Capturing test data

Each DataStage development team will already have a set of test data that they use to verify their flow's correct operation.  DataStage enables you to capture that data (regardless of whatever technology is currently used to supply it) and encapsulate it into a commonly managed, well governed artefact which can travel with your job to repeatedly assert its consistent behaviour in any downstream environment.  This existing test data can be captured by DataStage by running a flow in 'Data Interception' mode.

![screen capture](./images/ds-test-case-capture.png "test screen capture")

This process involves running your flow in a 'Capture' mode.  In this mode DataStage will interrogate the data flowing along the input and output links referenced in your test case specification and record the data it observes into the test data file defined (by your specification) for each link.  This permits the capture of structured and unstructured data from both batch and streaming data sources.

The data flowing along a flow's output links is captured as the current definition of 'expected' output into the relevant output data files.  When you alter the flow's functionality you may well need to [re-capture a new baseline](baselining-test-results.md) of expected results.

## Process

1. In the test case editor click **Intercept data**. You'll need to accept any 'Overwrite all test data' warning you receive by clicking **Intercept flow**.
1. You'll receive a message telling you the flow is running.  Select the **Test history** tab to browse the most recent history of test case job invocations, including jobs currently in progress.
1. Once the job is complete select the **Data** tab and browse the newly-populated test data under the the **Test data in use** tree.
