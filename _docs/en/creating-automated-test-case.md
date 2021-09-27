---
title: Creating Automated Test Case
permalink: /docs/en/creating-automated-test-case
key: docs-creating-automated-test-case
---
We are going to demo how to test a REST API that updates an article in database. Check section [Sample Test Case](#sample-test-case) if you are eager to see what the test case looks like.

The API is the sample Article API that is bundled with API Test Base. It does CRUD operations against the Article table in a sample H2 database. The sample database is automatically created under `<APITestBase_Home>/database` when API Test Base is launched for the first time.

We are planning to have three test steps in our test case
```
1. Set up database data
2. Invoke the API to update article
3. Check database data
```

## Create Test Case Outline
First of all, create a new test case. (You can create your preferred folder structure for managing test cases, by right clicking on folder and selecting needed context menu item.)

Now we can add test steps to the test case.

Under the Test Steps tab, click Create dropdown button and select Database Step. Enter the name of step 1 `Set up database data`. Click `Back` link to return to the test case edit view. Repeat this to add the other two test steps (one HTTP Step and one Database Step). The test case outline is created as shown below.

![Test Case Outline](../../screenshots/basic-use/test-case-outline.png)

## Populate Step 1
Click the name of step 1 to open its edit view.

Under the Endpoint Details tab, set JDBC URL `jdbc:h2:./database/sample;AUTO_SERVER=TRUE` which will be used by the test step to connect to the sample database. Here `./database/sample` in the URL equals to `<APITestBase_Home>/database/sample`, as API Test Base application is launched from directory `<APITestBase_Home>`. Then set Username and Password which can be found in `<APITestBase_Home>/config.yml`.

Under the Invocation tab, enter below SQL script.
```
-- Clear the table
delete from article;

-- Create two article records
insert into article (id, title, content) values (1, 'article1', 'content1');
insert into article (id, title, content) values (2, 'article2', 'content2');
```

Click the Invoke button to try it out (i.e. run the script), like shown below.

![Database Setup](../../screenshots/basic-use/database-setup.png)

Click the Back link to return to test case edit view.

## Populate Step 2
Click the name of step 2 to open its edit view.

Under the Endpoint Details tab, set URL `http://localhost:8090/api/articles/2`. Ignore Username and Password fields as they are not used in this demo.

Under the Invocation tab, select `PUT` from the Method dropdown list, click the menu dropdown button and select `Show HTTP Headers`.

In the grid above the Request Body text area, add a request HTTP header `Content-Type: application/json` using the Create item in the grid menu (located in the top right corner of the grid). We need this header in the request because the Article API requires it. If it is not provided, the invocation will see error response.

Input the request body for updating article 2.

```
{
  "id": 2,
  "title": "article2",
  "content": "Once upon a time ..."
}
```

Click the Invoke button to try it out and you'll see a successful response in the right pane.

Click the Assertions button to open the assertions pane. In the assertions pane, click Create dropdown button and select `StatusCodeEqual Assertion` to create a StatusCodeEqual assertion. Set the expected HTTP response status code (here 200), and click the Verify button to verify the assertion, as shown below.

![HTTP Invocation and Assertion](../../screenshots/basic-use/http-invocation-and-assertion.png)

More information about assertions can be found on this [page](/docs/en/assertions).

Click the Back link to return to the test case edit view.

## Populate Step 3
Click the name of step 3 to open its edit view.

Under the Endpoint Details tab, enter exactly the same information as in step 1 because we are interacting with the same database. The information duplication can be avoided by using `managed endpoints`. Refer to this [page](/docs/en/endpoints-management) for more details.

Under the Invocation tab, enter SQL query `select id, title, content from article;`.

Click the Invoke button to try it out (run the query), like shown below.

![Database Check Query Result](../../screenshots/basic-use/database-check-query-result.png)

Click the JSON View tab to see the JSON representation of the SQL query result set.

Click the Assertions button to open the assertions pane. In the assertions pane, click Create dropdown button and select `JSONEqual Assertion` to create a JSONEqual assertion. Copy the JSON string from the JSON View to the Expected JSON field. Click the Verify button to verify the assertion, as shown below.

![Database Check Query Result and Assertion](../../screenshots/basic-use/database-check-query-result-and-assertion.png)

Click the Back link to return to the test case edit view.

## Run the Test Case
Now we have finished editing our test case. It's time to run it. Click the Run button, and you'll see the result for the whole test case beside the Run button, and in the bottom pane an outline of result for all test steps, like shown below. Passed test step is indicated by green color and failed test step by red color.

![Test Case Run Result](../../screenshots/basic-use/test-case-run-result.png)

Click the link for a test step in the bottom pane to open a modal and see the step run report, like shown below.

![Test Step Run Report](../../screenshots/basic-use/test-step-run-report.png)

Click the result link beside the Run button to see the whole test case run report. This report can be saved as HTML file and used as test evidence in other places such as HP ALM.

## Sample Test Case
The test case created above is available for download at <a href="../../sample-testcases/basic-use/Update Article.json" download>sample test case</a>. After download, right click any folder on ATB UI and import it.