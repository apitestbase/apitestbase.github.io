---
title: Team Collaboration
permalink: /docs/en/team-collaboration
key: docs-team-collaboration
---
API Test Base application stores the requests, test cases, environments etc. you create in an ATB workspace under folder `<ATB_DATA_DIR>/fileplace/<the workspace>`. Refer to [Maintenance](/docs/en/maintenance) for more information.

Each request, test case, environment etc. is stored in a YAML file.

Here is a sample HTTP request YAML file

```
version: 1
type: "HTTP"
endpoint:
  type: "HTTP"
  url: "http://localhost:8090/api/articles"
apiRequest: !<HttpRequest>
  method: "GET"
otherProperties:
  timeout: "20"
```

You can use VCS (like Git) to share the YAML files to team members, and work on the requests, test cases, environments etc. as a team.

Recommended: share each ATB workspace as a separate VCS (like Git) repo.