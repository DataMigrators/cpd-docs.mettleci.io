# Unit Test Specification Format

- [Structure](#structure)
- [Given](#given)
-  - Sparse Lookup sources
- [When](#when)
- [Then](#then)
   - [Cluster keys](#cluster-keys)
   - Row count comparisons
   - Excluding columns from unit tests
- Test Specification Types
- Test specification patterns

## Structure
<a href="structure"></a>

A DataStage test case specification (often abbreviated ‘Spec') is a YAML-formatted file which uses a grammar modelled loosely on the [Gherkin syntax](https://cucumber.io/docs/gherkin/) used by the Cucumber testing tool. The overall structure follows the common Gherkin pattern …

```
given:
  # Use this input data as the source for the flow  
when:
  # Execute the test case with these parameter values
then:
  # Expect the flow to produce data that looks like this
```

## Given
<a href="given"></a>

The **given** section defined a list of stage nodes (or sparseLookup nodes, see below) defining input links whose values you wish to replace with test data. 

```
given:
  - stage: My_First_Sequential_File          # Stage 'My_First_Sequential_File' has...
    link: Input_Link                         # an output link 'Input_Link' which... 
    path: Sequential_File_1.csv              # will be injected with data from the test data file 'Sequential_File_1.csv'
  - stage: My_Sequential_Other_File
    link: Other_Input_Link
    path: Sequential_File_2.csv  
```

Each link is uniqely identified using a combination of the stage and link names (to uniquely identify an incoming link which supplies data to your Job) and a path node to identify the test data CSV file containing the test data that is to be injected on that incoming link.

### Sparse Lookup sources

When an input source is used with a Sparse Lookup stage then rather than using the stage node to specify the input you will use the sparseLookup node.

```
given:
  - stage: Some_Non_Lookup_Stage
    link: Some_Input_Link
    path: Some_File.csv
  - sparseLookup: SparseLookup
    path: Database-Reference.csv
    key:
      - KEY_COLUMN_1
      - KEY_COLUMN_2
```

The sparseLookup node specifies …

* the value defining the name of the sparse lookup reference stage,
* a path to the relevant CSV unit test data file, and
* a list of key columns to be used for the sparse lookup.

## When
<a href="when"></a>

The **when** node specifies which job will be executed during testing as well as any parameters (including job macros) that affect the data produced by the job.

```
when:
  job: KeyGeneratorExample                   # The DataStage flow with which this test spec is associated 
  parameters:
    DSJobStartDate: 2012-01-15               # Run the test using this value for the DSJobStartDate macro
    DSJobStartTime: 11:05:01                 # Run the test using this value for the DSJobStartTime macro 
    paramStartKey: 100                       # Run the test using this value for the paramStartKey Job parameter
```

## Then
<a href="then"></a>

The **then** section associates unit test data files with each of your flow's input links. 

```
then:
  # Compare output
  - stage: ODBC_customer
    link: customer_out
    path: ODBC_customers.csv
  - stage: ODBC_order
    link: order_out
    path: ODBC_orders.csv
    cluster:
      - ACCOUNT_ID
      - TYPE_CODE
```

Similar to the **Given** section, each link in the **Then** section is specified using a combination of  stage and link nodes (to uniquely identify an outgoing link which produces data from your flow) and a path node to identify the test data CSV file containing the test data that is to be injected on that incoming link.

### Cluster keys
<a href="cluster-keys"></a>


The cluster node is used to assist DataStage's resource management when using high volumes of test data.  Setting a **Cluster Key** will prompt DataStage to split the actual output and expected output using multiple, smaller subsets (based on the supplied keys) before the data is compared.  Data is split such that each subset will only contain records that have the same values for all columns that make up the Cluster Key.  In general, Cluster Keys should only be used when necessary and not specified by default. Read more about the using the cluster node in [High Volume Unit Tests](high-volume-unit-tests.md).

### Row count comparisons
<a href="row-count-comparisons"></a>

You can configure as test to only compare output row counts, rather than the content of those rows, by setting the **checkRowCountOnly** node to true.

```
then:
  - stage: Output
    link: Write1
    path: Output-Write1.csv
    checkRowCountOnly: true
```

**See this page for more information.**

### Excluding columns from unit tests

You can omit selected columns from the output comparison by listing them under an ignore node for the relevant output.

```
then:
  - stage: Transform
    link: Output
    path: Transform-Output.csv
    ignore: 
      - CREATION_DATE
      - LAST_UPDATED
```


## Test specification patterns
<a href="test-specification-patterns"></a>

Most DataStage flows can be tested simply by replacing input and output stages. However some flow designs may necessitate a more advanced testing configuration. The sections below outline DataStage test specification patterns that best match these job designs.

* [Testing stages with reject links]()
* [Testing Stored Procedure Stages]()
* [Testing Surrogate Key Generator Stages]()
* [Testing Sparse Lookup Stages]()
* [Testing Jobs with current date calculations]()
* [Tests featuring Local and Shared Containers]()

See also:

* [High volume Unit Tests](high-volume-unit-tests.md)

