Assertions are used in test step to verify that the API invocation response is as expected. 

There are currently Contains, XPath, XMLEqual, IntegerEqual, JSONPath, and JSONPathXMLEqual assertions that can be used in Iron Test. Most of them are common and easy to understand. Below are some special points to notice.

## XMLEqual Assertion
Used to verify that the actual (API response) XML is equal to the expected XML. 

You can use placeholders in the expected XML. A **placeholder** is used to specify that a text node in the expected XML is going to be compared to the text node in the actual XML not for equality but by a special rule. A placeholder is denoted by `#{...}`.
### Ignore
Suppose 

    expected XML "<elem1><elem11>#{irontest.ignore}</elem11></elem1>"

    actual XML "<elem1><elem11>abc</elem11></elem1>"

XMLEqual assertion will pass.

## JSONPath Assertion
Used to verify that the specified JSONPath evaluates to expected value against the actual (API response) JSON. 

For JSONPath syntax, please refer to [this page](https://github.com/jayway/JsonPath).

## JSONPathXMLEqual Assertion
Used to verify that the specified JSONPath evaluates to an XML document against the actual (API response) JSON, and the XML document is equal to the expected XML.

A common usage scenario of this assertion is in Database test step, when a SQL query reads a table column which is either XML type (like in SQL Server) or VARCHAR/CLOB type with XML document as value.

Placeholders can be used in the expected XML, like described in the [XMLEqual Assertion](https://github.com/zheng-wang/irontest/wiki/Assertions#xmlequal-assertion).