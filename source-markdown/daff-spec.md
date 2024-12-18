# Tabular diff specification

## Summary

Every DataStage test case has at least one test comparison which involves two sets of tabular data: An **Actual** result set, produced by your Flow, and an **Expected** result set, defined by your test case. One way to express the differences in these result sets is to describe the operations that would be required to modify the **Actual** data to match the `Expected` data. A description of the structural and content changes required between each **Actual** and **Expected** result set is called a **diff** as its output is loosely modelled after that of the [Unix 'diff' command](https://en.wikipedia.org/wiki/Diff).  

## Example test results

Test results are presented in tabular form, with the differences between the expected and actual data indicated in row headers, column headers, or cells.

![multiple differences](./images/ds-test-multi-diff.png "multiple differences")

The visual change indicators are intuitive and easy to follow:

| Change type             | Indicator | Example |
|-------------------------|-----------|---------|
| Inserted rows           | ![inserted row](./images/diff-row-insert.svg "inserted row header icon") | An additional, unexpected row (for customer 'Ardith Beahan') is present |
| Deleted rows            | ![deleted row](./images/diff-row-delete.svg "deleted row header icon") | The expected row (for 'Doc Brown') is missing |
| Inserted columns        | ![deleted column](./images/diff-column-insert.svg "deleted column header icon") | An additional, unexpected INTEGER column **CENTS** was produced |
| Deleted columns         | ![deleted column](./images/diff-column-delete.svg "delete column header icon") | The expected TINYINT column **MEMBERSHIP** was not found |
| Modified column metadata | Additional header row | A column was renamed from `FIRST_NAME` to `First_Name` as indicated by an additional header row |
| Modified cell values     | ![cell modified](./images/diff-difference.svg "cell modified indicator")        | A modified value (33298->33262) in the **DOLLARS** columns for person 'Josianne Mante'|
