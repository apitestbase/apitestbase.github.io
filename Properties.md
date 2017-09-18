## User Defined Properties
You can define properties on a test case through the Properties tab, then use them in the test steps of the test case. For example, define property Output_Queue_Name, and use it in an MQ test step through reference ${Output_Queue_Name}. When running the test step or test case, the properties are resolved to the associated values.

Property value is a string. If it contains line breaks, you can double click the property value cell in the grid to pop out a textarea to edit the value.

A straightforward usage is that a property can be defined on the test case once and used multiple times in the test steps.

Another usage is `pattern based test case creation`. If already familiar with a test pattern, you can define a test case as template to capture the test steps. You can then define properties on the template test case and reference them in the test steps. To create a test case, copy corresponding test case template, tailor the test steps as appropriate (such as removing unnecessary steps), enter the property values and the test case is ready to run. There is no need for you to dive into any test step to locate and enter those values. This treats the test case somewhat like a black box and properties like the arguments to the black box, hence increasing the speed of test case creation.
