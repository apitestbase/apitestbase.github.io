---
title: Data Driven Testing
permalink: /docs/en/data-driven-testing
key: docs-data-driven-testing
---
Data driven testing is a popular technique in API testing. It enables you to run the same test case with various test data.

Suppose you have a test case that invokes an API with input data and examines the API's output data. If you want to invoke the same API with different input data and examine corresponding output data, data driven testing can help. Define a data table on the test case, and each row in the table will trigger one individual run of the test case, feeding the run with properties in the row.

This technique saves you the need to create multiple test cases that use different test data but have the same test steps. It greatly reduces the cost of test case creation, run and maintenance when various test data is involved.

## Data Table
Data table can be defined on a test case under the Data Table tab of test case edit view. A sample is shown below.

Each row in the table contains a set of properties to feed each test case individual run. Property name is column name, and property value is cell value.
    
Property can be used in any test step or assertion. Refer to [Properties](/docs/en/properties) for more details.

A `Caption` column is by default defined in data table. It enables you to mark/label each row in the table, so that the purpose of the row is clear. Caption column will not be used as property when running test case, but it makes test case run report easier to read.

## Sample Scenario
Take the test case from [Creating Automated Test Case](/docs/en/creating-automated-test-case) as a starting point. We will refactor it to enable testing Article update with two sets of data in one test case. One is for testing successful article update, and the other is for testing an error case when the article title is too long (over 50 chars) to be persisted into the database.

Check section [Sample Test Case](#sample-test-case) if you are eager to see what the final test case looks like.

### Refactor the test case to be data driven
What we got from [Creating Automated Test Case](/docs/en/creating-automated-test-case) was a test case like below

![Basic Test Case](../../screenshots/basic-use/test-case-outline.png)

Firstly, we copy the `Update Article` test case, and rename the new test case to `Update Article - Data Driven`. These actions can be done by using the context menus (right click test case or folder) in the tree pane.

Secondly, we refactor test steps of the new test case to use property references.

Then we add data table rows on the new test case to define the properties.

#### Refactor step 2
Open step 2 `Invoke the API to update article` of the new test case.

Under the `Invocation` tab, replace the "title" field value in the request body with a property reference `${Input_Article_Title}`.

Click the `Assertions` tab (at bottom of screen) to open the assertions panel. Replace the Status Code value of the StatusCodeEqual assertion with a property reference `${Expected_API_Response_Status_Code}`.

For more thorough testing than only checking API response status code, we add a JSONEqual assertion to check the API response body. Set the Expected JSON with a property reference `${Expected_API_Response_JSON}`.

![Refactored Step 2](../../screenshots/data-driven-testing/refactored-step-2.png)

Notice that the properties `Input_Article_Title`, `Expected_API_Response_Status_Code` and `Expected_API_Response_JSON` do not exist yet. We'll create them in data table later.

#### Refactor step 3
Open step 3 `Check database data` of the new test case.

Replace the Expected JSON of the JSONEqual assertion with a property reference `${Expected_Result_Database_Data}`.

![Refactored Step 3](../../screenshots/data-driven-testing/refactored-step-3.png)

#### Add data table columns
We have used property references in our test steps. Now we'll create the properties in data table.

Open the new test case, and under the `Data Table` tab click the `+ Column` button to add a new column. Rename it (via column header menu) to `Input_Article_Title`. Similarly, add more columns for the other properties.

![Data Table with Columns Only](../../screenshots/data-driven-testing/data-table-with-columns-only.png)

#### Add data table rows
Under the `Data Table` tab, use the `+ Row` button to add two rows, and fill the rows with below data

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Caption&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Input_Article_Title | Expected_API_Response_Status_Code | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Expected_API_Response_JSON&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Expected_Result_Database_Data |
| --- | --- | --- | --- | --- |
| Positive | article2 | 200 | {<br>&nbsp;&nbsp;"id": 2,<br>&nbsp;&nbsp;"title": "article2",<br>&nbsp;&nbsp;"content": "Once upon a time ..."<br>} | [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID":&nbsp;1,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"TITLE":&nbsp;"article1",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"CONTENT":&nbsp;"content1"<br>&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID":&nbsp;2,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"TITLE":&nbsp;"article2",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"CONTENT":&nbsp;"Once&nbsp;upon&nbsp;a&nbsp;time&nbsp;..."<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>] |
| Negative - article title too long | looooooooooooooo ooooooooooooo oooooooooooong title | 500 | {<br>&nbsp;&nbsp;"code": 500,<br>&nbsp;&nbsp;"message": "#{json-unit.ignore}",<br>&nbsp;&nbsp;"details": "#{json-unit.regex}.\*Value too long for column \\"TITLE[\\\\s\\\\S]\*"<br>} | [<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID":&nbsp;1,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"TITLE":&nbsp;"article1",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"CONTENT":&nbsp;"content1"<br>&nbsp;&nbsp;&nbsp;&nbsp;},<br>&nbsp;&nbsp;&nbsp;&nbsp;{<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"ID":&nbsp;2,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"TITLE":&nbsp;"article2",<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"CONTENT":&nbsp;"content2"<br>&nbsp;&nbsp;&nbsp;&nbsp;}<br>] |

If you don't understand what `#{json-unit.ignore}` or `#{json-unit.regex}` means, refer to [JSONEqual Assertion](/docs/en/assertions#jsonequal-assertion).

In the data table, click a cell to fill short (typically one line) data, or double click a cell to bring up a modal for filling long (typically multi-line) value like below.

![Filling Multi-line Value](../../screenshots/data-driven-testing/filling-multi-line-value.png)

The complete data table looks like below.

![Complete Data Table](../../screenshots/data-driven-testing/complete-data-table.png)

Now we have finished refactoring the test case. The testing logic is not changed. The only thing changed is that the test case is now data driven.

### Run the test case
Finally, it's time to run the test case. Click the `Run` button on the test case edit view, and you'll see the result for the whole test case beside the Run button, and in the right pane an outline of result for all individual runs. Click an individual run to expand it and view the result of its step runs.

![Data Driven Test Case Run](../../screenshots/data-driven-testing/data-driven-test-case-run.png)

Click a step run link to view its report, or click the result link beside the Run button to see the whole test case run report.

## Data Driven Test Step
Similar to test case level data table, test step level data table enables the test step to be run one or more times during one test case run. This enables the test step to do repeated work driven by a data table, without duplicating the test step definition in the same test case.

## Sample Test Case
The final test case created above is available for download at <a href="../../sample-testcases/data-driven-testing/Update Article - Data Driven.json" download>sample test case</a>. After download, right click anywhere in the left side pane on ATB UI, and select `Import Test Case` to import it.