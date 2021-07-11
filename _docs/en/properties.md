---
title: Properties
permalink: /docs/en/properties
key: docs-properties
---
A property in Iron Test is a named String or Endpoint (currently only DB endpoint is supported). A String property can be used in any test step, assertion or HTTP stub with reference `${<Property_Name>}`, e.g. ${Output_Queue_Name}. A (DB) Endpoint property can only be used in a DB test step with reference `<Property_Name>` (not enclosed with ${}).

When running the test step or test case, the properties are resolved to the associated String values or Endpoint objects.

Nested String property, i.e. property inside property value, is supported. For example: define Prop1="Hello" and Prop2="${Prop1} World!", and Prop2's value will be "Hello World!" when running the test case.

## User Defined Properties
You can define custom String properties on a test case through the Properties tab, then use them in the test steps and assertions of the test case. If the property value contains line breaks, you can double click the property value cell (after entering edit mode) in the grid to pop out a textarea to edit the value.

A straightforward usage of user defined property is that a property can be defined on the test case once and used multiple times in the test steps or assertions.

Another usage is `pattern based test case creation`. If already familiar with a test pattern, you can define a test case as template to capture the test steps. You can then define properties on the template test case and reference them in the test steps. To create a test case, copy corresponding test case template, tailor the test steps as appropriate (such as removing unnecessary steps), enter the property values and the test case is ready to run. There is no need for you to dive into any test step to locate and enter those values. This treats the test case somewhat like a black box and properties like the arguments to the black box, hence increasing the speed of test case creation.

## Implicit Properties
These are String properties dynamically created by Iron Test when running a test step or test case.

#### Test_Case_Start_Time
The timestamp when the test case run starts. Format is yyyy-mm-dd hh:mm:ss.fff, e.g. 1997-01-31 09:26:50.124.

#### Test_Step_Start_Time
The timestamp when the test step run starts. Format is same as Test_Case_Start_Time.

#### Test_Case_Individual_Start_Time
The timestamp when the test case individual run (like in data driven test case run) starts. Format is same as Test_Case_Start_Time.

## Data Table
Properties can also come from a data table. Refer to [Data Driven Testing](https://github.com/zheng-wang/irontest/wiki/Data-Driven-Testing).

## Extracted Properties
Extract properties from API response in one test step, and use them in later test steps during test case run. This enables passing dynamic data between test steps.

Extracted property value is always a string.

Currently only HTTP test step has property extractors.

### JSONPath Property Extractor
Used to extract property from HTTP response body via JSON path.

[![JSONPath Property Extractor](https://github.com/zheng-wang/irontest/blob/master/screenshots/properties/jsonpath-property-extractor.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/properties/jsonpath-property-extractor.png)

### Cookie Property Extractor
Used to extract property from HTTP response Set-Cookie header by cookie name. 

[![Cookie Property Extractor](https://github.com/zheng-wang/irontest/blob/master/screenshots/properties/cookie-property-extractor.png)](https://github.com/zheng-wang/irontest/blob/master/screenshots/properties/cookie-property-extractor.png)