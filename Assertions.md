Assertions are used in test step to verify that the API invocation response is as expected. 

There are currently Contains, XPath, XMLEqual, IntegerEqual, JSONPath, and JSONPathXMLEqual assertions that can be used in Iron Test. Most of them are common and easy to understand. Below are some special points to notice.

## XMLEqual
This assertion is used to verify that the actual (API response) XML is equal to the expected XML. You can use placeholders in the expected XML. A **placeholder** is used to specify that a text node in the expected XML is going to be compared to the text node in the actual XML not for equality but by a special rule.
### Ignore
Suppose 

    expected XML "<elem1><elem11>#{irontest.ignore}</elem11></elem1>"

    actual XML "<elem1><elem11>abc</elem11></elem1>"

XMLEqual assertion will return true.