---
title: Assertions
permalink: /docs/en/assertions
key: docs-assertions
---
Assertions are used in test step to verify that the API invocation response is as expected.

Assertions can also be used as ad hoc utilities. For example: if you want to validate a JSON against a JSON Schema, create an HTTP step in an ad hoc test case, and use the JSONValidAgainstJSONSchema assertion directly. There is no need to run the test step or test case if all you want is just to validate a JSON manually. Frequent ad hoc usages:
* Validate JSON against JSON Schema
* Validate XML against XSD(s)
* Compare two XML/JSON for equality
* Evaluate XPath/JSONPath against XML/JSON

There are currently Contains, XPath, XMLEqual, JSONEqual, etc. assertions that can be used in API Test Base. Most of them are common and easy to understand. Below are some special points to notice.

## XMLEqual Assertion
Used to verify that the actual (API response) XML is equal to the expected XML. 

The underlying diff engine is XMLUnit, and you can use xmlunit placeholders in the expected XML. An XMLUnit **placeholder** is used to specify that a text node (element content or attribute value) in the expected XML is going to be compared to the text node in the actual XML not for equality but by a special rule. The only notice here is that the placeholders must be denoted by `#{...}`, because `${...}` has been reserved for API Test Base [Properties](/docs/en/properties) usage.

Currently supported placeholders: `#{xmlunit.ignore}`, `#{xmlunit.isNumber}`, `#{xmlunit.isDateTime(format)}`, `#{xmlunit.matchesRegex(regex)}`.

Example:

Given

    expected XML: <elem1><elem11>#{xmlunit.ignore}</elem11></elem1>

    actual XML: <elem1><elem11>abc</elem11></elem1>

XMLEqual assertion will pass.

## XMLValidAgainstXSD Assertion
Used to verify that the actual (API response) XML conforms to specified XML Schema.

The assertion supports validating against a single XSD, or a zip containing multiple XSDs with folder structures and import/include relationship.

## JSONEqual Assertion
Used to verify that the actual (API response) JSON is equal to the expected JSON. 

The underlying diff engine is JsonUnit, and you can use json-unit placeholders in the expected JSON, like `#{json-unit.ignore}`, `#{json-unit.any-number}`, `#{json-unit.any-string}`, `#{json-unit.any-boolean}`, `#{json-unit.regex}[A-Z]+`. More details can be found at [JsonUnit Readme](https://github.com/lukas-krecan/JsonUnit). The only notice here is that the placeholder must be denoted by `#{...}`, because `${...}` has been reserved for API Test Base [Properties](/docs/en/properties) usage.

Given

    expected JSON: {"a": 1, "b": "#{json-unit.ignore}"}
    
    actual JSON: {"a": 1, "b": "xyz" }

JSONEqual assertion will pass.

## JSONPath Assertion
Used to verify that the specified JSONPath evaluates to expected value against the actual (API response) JSON. 

For JSONPath syntax, please refer to [this page](https://github.com/jayway/JsonPath).

## JSONPathXMLEqual Assertion
Used to verify that the specified JSONPath evaluates to an XML document against the actual (API response) JSON, and the XML document is equal to the expected XML.

A common usage scenario of this assertion is in Database test step, when a SQL query reads a table column which is either XML type (like in SQL Server) or VARCHAR/CLOB type with XML document as value.

Placeholders can be used in the expected XML, as described in the [XMLEqual Assertion](#xmlequal-assertion).

## Plain Text Assertions
Used to verify the actual (API response) text is as expected.

There are Contains, TextEqual, Substring and RegexMatch assertions in this category.