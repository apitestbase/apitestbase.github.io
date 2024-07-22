---
title: Endpoints Management
permalink: /docs/en/endpoints-management
key: docs-endpoints-management
---
When a new request or test step is created, and it needs an endpoint, an `unmanaged endpoint` with empty values will be created and associated with it.

Unmanaged endpoint is specific to a request or test step, and is invisible to other requests or test steps.

To reuse endpoints across requests or test steps, you can create `managed endpoints`.

## Create Environment
Managed endpoints reside in environments.

To create an environment, click the `Environments` icon in the left side bar of the screen, right click the blank space in the left side pane and select `New Environment` from the context menu.

Give it a name (like 'Local'), press Enter, and a new empty environment is created with its edit view opened.

![Environment](../../screenshots/env-mgmt/environment.png)

There are two ways to create a managed endpoint. Here we use HTTP endpoint as an example.

## Create Managed Endpoint in the Environments area
In the 'Local' environment we just created, click `+ Endpoint` dropdown button, select `HTTP`, give it a name (like 'Article API') and press Enter. A managed HTTP endpoint is created with its edit view displayed on the right.

Input details into the fields, and the endpoint looks like below. 

![Managed HTTP Endpoint](../../screenshots/env-mgmt/managed-http-endpoint.png)

To use the newly created managed endpoint, go to an HTTP request or test step, click the `Endpoint` tab, and click `Select Managed Endpoint` button to see a modal that lists all HTTP endpoints. 

![Select Managed Endpoint](../../screenshots/env-mgmt/select-managed-endpoint.png)

Click the endpoint name to select it for use in the HTTP request or test step.

## Share Unmanaged Endpoint from Test Step
This is a convenient feature for you to capture endpoint details while editing a request or test step, and then turn the (unmanaged) endpoint into managed.

Under `Endpoint` tab of a request or test step, click `Share Endpoint` button. Enter details and click `OK` button. The (unmanaged) endpoint will be shared into the specified environment (here 'Local').

![Share Unmanaged Endpoint](../../screenshots/env-mgmt/share-unmanaged-endpoint.png)

## Changing from Managed Endpoint to Unmanaged for a Request or Test Step
Clicking the `Unmanage Endpoint` button under the `Endpoint` tab will allow you to clone the managed endpoint to an unmanaged one for the request or test step. The managed one stays untouched in the environment.
 