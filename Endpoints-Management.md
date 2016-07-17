When we entered SOAP Address under the SOAP test step Endpoint Details tab, we were using unmanaged endpoint. Unmanaged endpoint is specific to a test step. It is invisible to other test steps in the same test case, or other test cases.

To reuse endpoints across test steps or test cases, you can create managed endpoints. Managed endpoint resides in environment. If you haven't created an environment, click the Administration > Environments link in the left panel, and click Create button. A new environment is created and its edit view displays.
 
Under the Basic Info tab, enter name and (optional) description. Click Endpoints tab.

[[https://github.com/zheng-wang/irontest/blob/master/screenshots/env-mgmt/environment.png|alt=environment]]

There are two ways to create a managed endpoint.

# Create Managed Endpoint in the Environments area
In the 'Local' environment we just created, click Create dropdown button and select SOAP Endpoint to create a managed SOAP endpoint. SOAP endpoint edit view displays. Enter details and Iron Test saves them automatically.

[[https://github.com/zheng-wang/irontest/blob/master/screenshots/env-mgmt/managed-soap-endpoint.png|alt=Managed SOAP Endpoint]]

To use the newly created managed endpoint, go to the SOAP test step by clicking its link in the test case edit view, and click the Endpoint Details tab. Click Select Managed Endpoint button to see a SOAP endpoint list popup. 

[[https://github.com/zheng-wang/irontest/blob/master/screenshots/env-mgmt/select-managed-endpoint.png|alt=Select Managed Endpoint]]

Click the endpoint name to select it for use in the SOAP test step.