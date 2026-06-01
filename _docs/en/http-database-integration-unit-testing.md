---
title: HTTP-Database Integration Unit Testing
permalink: /docs/en/http-database-integration-unit-testing
key: docs-http-database-integration-unit-testing
---
## Introduction To The API Under Test
Suppose you want to test an Article API that exposes an HTTP endpoint. When a client invokes it with details via HTTP PUT, it updates an article record in a SQL Server database table.

![Article API](../../screenshots/http-db/article-api.png)

## Test Isolation
To integration unit test the Article API, you would set up and use a local stub database. This has some benefits

* The test includes testing the side effect of the Article API. That is, the API interacts with a real database, and actually results in the database data being changed.
* The test is fully controlled by you on your local machine, without impacting any other developer.

![Article API Test Isolation](../../screenshots/http-db/article-api-test-isolation.png)

### Set up Stub Database Instance
The simplest way of spinning up a local SQL Server instance is probably launching a Docker container.

This can be done through ATB's Docker test step.

### Create Stub Database and Table
The Article API will write into the dbo.Article table in the Article database. We need a DDL script that creates the database and the table.

Some common ways to obtain or draft the DDL script are
* (Most likely case) If the table is already created in a shared database, like the one in SIT environment, use a database client (like DBeaver) to connect to the database and get the table DDL.
* Get the script from the database administrator.
* If the table is only defined in a design document, manually create the script based on the document content.

Tailor the script, like by removing unneeded table columns, to suit your testing need.

Below is a sample DDL script
```
    IF DB_ID('Article') IS NULL
        CREATE DATABASE Article;
    
    IF OBJECT_ID('Article.dbo.Article', 'U') IS NOT NULL
        DROP TABLE Article.dbo.Article;
    
    CREATE TABLE [Article].[dbo].[Article] (
        ID INT,
        Title VARCHAR(50),
        Content VARCHAR(5000)
    );
```

The script can be run through ATB's Database test step.

### Use Stub Database Endpoint
Design the API to point to different dependencies in different environments. This is normally achieved by setting different dependency endpoint addresses in different property files. Each property file contains all properties for the API for a specific environment like Dev/Test/QA/Prod.

When deploying and running the API, dynamically load the property file for that specific environment.

Here is a sample property file `article-api-dev.properties` for a developer's local machine.
~~~
    database.host=localhost
    database.port=1433
    database.name=Article
    database.username=<encrypted username>
    database.password=<encrypted password>
~~~

## Database Test Data Preparation
We have created the stub table, but it is empty. Each test case will need to populate the table with corresponding test data. Effectively, we need some SQL `INSERT` statements for this purpose.

Some common ways to obtain or draft the `INSERT` statements are
* If there are already some rows in a shared database, like the one in SIT environment, use a database client (like DBeaver) to export or generate `INSERT` statements out of the rows. Tailor them, like by removing unneeded columns, to suit your testing need.
* Given we already got the DDL, it shouldn't be hard to manually craft the `INSERT` statements.
* Use a database client (like DBeaver) to manually run the DDL script to create the table in the local stub database, insert some dummy rows, and export or generate the `INSERT` statements. 

The `INSERT` statements can be run through ATB's Database test step.

## Test Case Creation
Refer to [Creating Automated Test Case](/docs/en/creating-automated-test-case). On that page, the database is an H2 database, but the steps for test case creation and run can be similarly applied here.

Just remember to
* Include the steps for setting up the local stub database.
* Use the local stub database endpoint (instead of the H2 one) in the test case.
* Use your own local Article API endpoint (instead of the ATB bundled one).
* Make sure SQL Server JDBC libraries are copied to ATB lib folder, as described [here](/docs/en/interact-with-other-systems#sql-server)

## Sample Test Case
The test case created above is available for download at <a href="../../sample-testcases/http-db/Positive.json" download>sample test case</a>.

After download, right click anywhere in the left side pane on ATB UI, and select `Import Test Case` to import it.

## What is Integration Unit Testing?
Refer to [this post](https://medium.com/@zhengwang666/integration-unit-testing-683fbf995c43){:target="_blank"}.