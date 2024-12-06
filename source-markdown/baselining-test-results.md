# Baselining a test result

As the logic of your flow changes, so the test data files representing your desired output will also need to change.  DataStage can automatically re-baseline your expected output based on an execution of your DataStage job.  Note that while this process is similar to the process described in [Capturing test data](capturing-test-data.md) this process differs in that it only captures the **output(s)** of your job. 

![representation of a DataStage job showing CSV files being injected into source stages and an output stage referngin a CSV file which does not exist](./images/ds-test-case-baseline-output.png "test baseline screen capture")

# Re-capturing test case results

1. In the test case editor, open the **Test data in use** tree and select the test output file(s) you want to re-baseline.
1. In the data area above the table click the **trash** icon to delete the data file.
1. Your test specification now refers to at least one CSV data file which no longer exists. [Execute your test case](executing-datastage-test-cases.md) without changing the specification. DataStage will identify that no expected results exist for the file(s) you’ve deleted and re-create them using the data captured at runtime.  Note that test case will fail when executed in this mode.
1. Re-execute the same test case job.  DataStage will now use the new baseline results files and your test case will pass. 

 

 