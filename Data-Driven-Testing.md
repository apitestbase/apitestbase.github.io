Data driven testing is a popular technique in API testing. It enables you to run the same test case with various test data.

Suppose you have an API test case that invokes the API with input data and examines the API's output data. If you want to invoke the same API with different input data and examine corresponding output data, data driven testing can help. Define a data table on the test case, and each row in the table will trigger one individual run of the test case, feeding the run with properties in the row.

This technique saves you the need to create multiple test cases that use different test data but have the same test steps. It greatly reduces the cost of test case creation, run and maintenance when various test data is involved.

## Data Table
Data table in Iron Test can be defined on a test case under the Data Table tab of test case edit view. A sample is shown below.

Each row in the table contains a set of properties to feed each test case individual run. Property name is column name, and property value is cell value.
    
You can define two types of columns (properties) in the table: `String column, DB endpoint column`.

String property can be referenced in any test step or assertion, and DB endpoint property can only be referenced in DB test step. Refer to [Properties](https://github.com/zheng-wang/irontest/wiki/Properties) for more details.

A `Caption` column is by default defined in data table. It enables you to mark/label each row in the table, so that the purpose of the row is clear. Caption column will not be used as property when running test case, but it makes test case run report easier to read.

## Sample Scenario
Take the test case from [basic use](https://github.com/zheng-wang/irontest#integrated-soap-web-service-testing) as a starting point. We will refactor it to enable testing two operations of the Article web service in one test case.

### Refactor the test case to be data driven
What we get from the [basic use](https://github.com/zheng-wang/irontest#integrated-soap-web-service-testing) is a test case like below

[![Basic Test Case](https://github.com/zheng-wang/irontest/blob/master/screenshots/integrated-soap-testing/test-case-outline.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/integrated-soap-testing/test-case-outline.png)

Firstly, we rename test steps to be operation agnostic.

[![Test Case with New Step Names](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/test-case-with-new-step-names.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/test-case-with-new-step-names.png)

Then we go to the Data Table tab and add a row by clicking the Add Row button. Click the Caption cell in the new row and set its value as `updateArticleByTitle`.

[![Initial Data Table Row](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/initial-data-table-row.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/initial-data-table-row.png)

Go back to Test Steps tab and open the `Call the web service operation` step. Cut the content under request body (save it somewhere else temporarily), and fill in with a property reference ${Request_Body_Content}. The property Input_Body_Content does not exist. We'll create it in data table shortly.

[![New Request Message](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/new-request-message.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/new-request-message.png)

Go to the Data Table tab of the test case, click the Add Column > String Column button to add a new column, and set its name Request_Body_Content. This is the property that'll be used by the `Call the web service operation` step. Double click the property cell to bring up a modal, and paste the content we just cut.

[![Request Body Content for Article Update](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/request-body-content-for-article-update.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/request-body-content-for-article-update.png)

Similarly, we refactor the assertion of the `Check database data` step, cutting the original Expected JSON (save it somewhere else temporarily), and filling in with property reference ${Expected_Result_Records}.

[![New Assertion](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/new-assertion.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/new-assertion.png)

Go to the Data Table tab of the test case, add a new String column Expected_Result_Records, double click the property cell to bring up a modal, and paste the content we just cut.

Now we have finished refactoring the test case. The testing logic is not changed. The only thing changed is that the test case is now data driven.

[![Finished Data Table Row for Article Update](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/finished-data-table-row-for-article-update.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/finished-data-table-row-for-article-update.png)

### Add new test data
We can drive the test case with more test data. Here we add a data table row for the `deleteArticleById` operation of the Article web service, so that we can test the operation with the same test case. This time, there is no need to revisit any test step.

[![Finished Data Table Rows](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/finished-data-table-rows.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/finished-data-table-rows.png)

### Run the test case
Finally, it's time to run the test case. Click the Run button on the test case edit view, and you'll see the result for the whole test case beside the Run button, and in the bottom pane an outline of result for all individual runs. Click an individual run to expand it and view the result of its step runs.

[![Data Driven Test Case Run](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/data-driven-test-case-run.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/data-driven-testing/data-driven-test-case-run.png)

Click a step run link to view its report, or click the result link beside the Run button to see the whole test case run report.

### Summary
With data driven testing, all operations of the Article web service can be tested by one test case. Similarly, testing of one operation with various (typically edge) parameters can also be included in the test case.