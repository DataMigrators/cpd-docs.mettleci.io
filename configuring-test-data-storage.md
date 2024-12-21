# Configuring test data storage

## Create a connection

Start by creating a connection to a storage volume where test assets will be stored.

1. From the CPD Cluster home page open an existing project or create a new project then, on the **Assets** tab, click ***New Asset** > **Connect to a data source**.
1. On the resulting page select **Storage column** and **Next**.
1. On the **Create connection: Storage volume** page give your new connection a name and optional description.
1. Select an available NFS volume. If you don't already have an NFS volume available ...
   1. click **New volume**.
   1. Select a namespace (the default will be fine).
   1. Give your storage volume a name and optional description.
   1. Select your preferred storage type (e.g. **External NFS**) and click **Add**.
1. Under Personal credentials > Input method select **Enter credentials manually**.
1. Check the **Use my platform login credentials** then click **Test connection**.
1. Assuming you have a successful connection, click **Save**.
<!-- if the connection is NOT successfull, what is fhe most likely issue -->

## Add the connection to your project settings

Now you'll configure DataStage to use your storage volume for storing test case assets.

1. From the CPD Cluster home page select the **Manage** tab then, under **Tools**, select **DataStage** and then select the tab **Test cases**.
1. For **Test data connection type** select **Storage Volume**.
1. For **Test data storage** select the name of the Storage volume you created in the step above.
1. For **Default DataStage Test case job suffix** specify a suffix which will be appended to all test case jobs to help distinguish them in the job log from invocations of their associated flows. e.g. ` DataStage TestCase Job`. Note the leading space included here, as DataStage will not automatically add one for you.
<!-- Does this mean the actual text ' Datastage TestCase Job' will be appended to all test case jobs? -->
