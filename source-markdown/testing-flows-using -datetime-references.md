# Testing flows using date/time references

Repeatable DataStage tests require that the flow being tested produces deterministic data based on a set of predefined inputs and parameters.  Calculations based on the current date or time are common in DataStage flow designs but will cause the output produced by those flows to change depending on the date and time when its job is executed.  This page outlines practices aimed at ensuring jobs using current date calculations are able to be validly tested.

# Transformer Stages using system time and date functions

Transformer stages with derivations using the `CurrentDate()`, `CurrentTime()` or `CurrentTimestamp()` functions need to be considered carefully whn designing your tests. Unless your calculation requires a specific date and/or time then calls to the standard `CurrentDate()`, `CurrentTime()` and `CurrentTimestamp()` functions could potentially be substituted with calls to the `DSJobStartDate`, `DSJobStartTime` and `DSJobStartTimestamp` macros. This enables you to subsitute them with a specific value during testing.  Add `DSJobStartDate`, `DSJobStartTime`, and `DSJobStartTimestamp` (as required) to the `parameters` clause of the test Specification's `when` node and set the appropriate date and time values used during Unit Testing.

## Example

Open image-20200130-051121.png
Screenshot of Transformer configuration UI showing use of date macro in Derivation.

```
...
when:
  parameters:
    DSJobStartDate: 2012-01-15
    DSJobStartTime: 11:05:01
```

Be careful when setting DSJobStartTimestamp in conjunction with either DSJobStartDate or DSJobStartTime, the MettleCI Unit Testing feature does not enforce that these parameters are logically consistent.

Ignoring columns which are known to be non-deterministic
Rather than ensuring that a job under test produces deterministic data, you may decide to exclude one or more output columns from Unit Test comparisons.  This can be done by adding the columns to be ignored to the then clause of the Unit Test Specification.

## Example

Open image-20200203-021938.png
DataStage Classic Designer Client screenshot showing columns to be ignored in the Unit Test.
 
```
given:
...
when:
...
then:
  - stage: Transform
    link: Output
    path: Transform-Output.csv
    ignore: 
      - CREATION_DATE
      - LAST_UPDATED
```

**Danger**: Ignoring columns will prevent columns containing non-deterministic from affecting test results.  This will also omit those columns from test comparisons, so unexpected output in those columns, or changes in the output of those columns, will not be detected.

 

