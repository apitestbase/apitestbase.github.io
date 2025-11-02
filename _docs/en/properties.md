---
title: Properties
permalink: /docs/en/properties
key: docs-properties
---
A property in API Test Base is a named String.

A property can be defined in various ways and scopes, and used in request, test case, test step, assertion or HTTP stub with syntax `${<Property_Name>}`, e.g. ${Output_Queue_Name}.

## User Defined Properties
You can define custom properties in a request, test case, environment or workspace on the `Properties` tab, then use/reuse them across the request, test case, environment or workspace.

Another usage is `template based test case creation`. If already familiar with a test pattern, you can create a test case as template to capture the test steps. Define properties on the template test case and use them in the test steps. To create a new test case, copy the test case template, tailor the test steps as appropriate (such as removing unnecessary steps), enter the property values and the new test case is ready to run. There is no need for you to dive into any test step to locate and enter those values. This treats the test case like a black box and properties like the parameters to the black box, hence making it easier to create a new test case.

## Implicit Properties
These are properties dynamically created by ATB.

#### ATB_Data_Dir
The file system directory where all your test data and configurations are located. Refer to [Administration](/docs/en/administration) for more details.

#### ATB_HTTP_Port
The HTTP port number exposed by ATB for its REST APIs and UI. It can be configured as seen in [Administration](/docs/en/administration).

#### Auto_Mock_Server_HTTP_Port
The HTTP port number of the `Auto` mock server in the current workspace.

#### Manual_Mock_Server_HTTP_Port
The HTTP port number of the `Manual` mock server in the current workspace.

#### Test_Case_Start_Time
The timestamp when the test case run starts. Format is yyyy-mm-dd hh:mm:ss.fff, e.g. 1997-01-31 09:26:50.124.

#### Test_Step_Start_Time
The timestamp when the test step run starts. Format is same as Test_Case_Start_Time.

#### Test_Case_Individual_Start_Time
The timestamp when the test case individual run (like in data driven test case run) starts. Format is same as Test_Case_Start_Time.

#### Test_Step_Repeat_Run_Index
Index of repeat run when a test step is defined as a repeated test step and it is run in a test case. Starting from 1.

## Data Table
Properties can also come from a data table. Refer to [Data Driven Testing](/docs/en/data-driven-testing).

## Extracted Properties
Extract properties from API response in one test step, and use them in later test steps during test case run. This enables passing dynamic data between test steps.

Currently only HTTP test step has property extractors.

### JSONPath Property Extractor
Used for extracting property from HTTP response body via JSON path. For example:

![JSONPath Property Extractor](../../screenshots/properties/jsonpath-property-extractor.png)

### Cookie Property Extractor
Used for extracting property from HTTP response Set-Cookie header by cookie name. For example:

![Cookie Property Extractor](../../screenshots/properties/cookie-property-extractor.png)

### XPath Property Extractor
Used for extracting property from HTTP response body via XPath. The response body can be XML or HTML.

When the response body is HTML, the XPath evaluation is powered by [JsoupXpath](https://github.com/zhegexiaohuozi/JsoupXpath).

Some tips on JsoupXpath:
- In absolute XPath, root node is `<html>`. For example, to select an element, you can use an absolute XPath like `/body/form`, but not `/html/body/form`.
- XML namespace is not applicable.

## Nested Properties
Nested property, i.e. property inside property value, is supported. For example:
```
Prop1="Hello"
Prop2="${Prop1} World!"        // Prop2's value is "Hello World!"
```