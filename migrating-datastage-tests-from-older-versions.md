# Migrating test cases from older versions of DataStage

Users of IBM® DataStage® v11.x who built DataStage unit tests using [MettleCI](https://www.mettleci.com/) will be familiar with the YAML syntax used to specify those unit tests. Test cases for Cloud Pak DataStage are different in that they are specified using [JSON](test-specification-format.md) rather than YAML.  Despite this syntactic difference, the options and structure of a test specification remain identical.

Existing MettleCI unit tests are easily migrated into DataStage on Cloud Pak for Data using the `mettleci unittest migrate` command available in the MettleCI command line.  The [documentation](https://datamigrators.atlassian.net/wiki/spaces/MCIDOC/pages/2851274753/UnitTest+Migrate+Command) for this command describes the process you need to undertake to safely migrate your tests to Cloud Pak.
