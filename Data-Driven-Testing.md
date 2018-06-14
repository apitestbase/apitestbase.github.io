Data driven testing is a popular technique in API testing. It enables you to run the same test case with various test data.

Suppose you have an API test case that invokes the API with input data and examines the API's output data. If you want to invoke the same API with different input data and examine corresponding output data, data driven testing can help. Define a data table on the test case, and each row in the table will trigger one individual run of the test case, feeding the run with properties in the row.

This technique saves you the need to create multiple test cases that use different test data but have the same test steps. It greatly reduces the cost of test case creation, run and maintenance when various test data is involved.

## Data Table
Data table in Iron Test can be defined on a test case under the Data Table tab of test case edit view. A sample is shown below.

Each row in the table contains a set of properties to feed each test case individual run. Property name is column name, and property value is cell value.
    
You can define two types of columns (properties) in the table: `String column, DB endpoint column`.

String property can be referenced in any test step, in the same way as [Properties](https://github.com/zheng-wang/irontest/wiki/Properties). DB endpoint property can only be referenced in DB test step, using syntax <Property_Name> (not enclosed by ${}).

A `Caption` column is by default defined in data table. It enables you to mark/label each row in the table, so that the purpose of the row is clear. Caption column will not be used as property when running test case, but it makes test case run report easier to read.

## Sample Scenario
Take the test case from [basic use](https://github.com/zheng-wang/irontest#integrated-soap-web-service-testing) as a starting point. We will update it to enable testing two operations of the Article web service in one test case.

With data driven testing, all operations of the Article web service can be tested by one test case. Similarly, testing of one operation with various (typically edge) parameters can also be included in the test case.