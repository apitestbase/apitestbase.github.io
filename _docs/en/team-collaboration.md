---
title: Team Collaboration
permalink: /docs/en/team-collaboration
key: docs-team-collaboration
---
An API Test Base (ATB) workspace is a folder of plain files on disk, so an ordinary VCS (like Git) is all your team needs to collaborate on it.

ATB stores the requests, test cases, environments, etc. you create in a workspace under folder `<ATB_DATA_DIR>/fileplace/<workspace name>`. Refer to [Administration](/docs/en/administration) for more information.

Each request, test case, environment, etc. is stored in its own YAML file.

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

The files are plain, human-readable YAML, so changes produce meaningful diffs and can be reviewed like any code change - for example, through pull requests.

You can use VCS (like Git) to share the YAML files with team members, and work on the requests, test cases, environments, etc. as a team.

Because each item is stored in its own file, teammates working on different items rarely touch the same file, so merge conflicts are uncommon - and when one does occur, it is resolved like any ordinary text conflict.

When the YAML files change on disk - for example, after a `git pull` of teammates' work, or after you edit a file manually - a running ATB picks up the saved changes automatically and reflects them on the ATB UI. No restart is needed.

Note that [secret](/docs/en/environments-management) values do not travel with the workspace. Wherever a secret is used - a named secret in an environment, or an unnamed secret in a managed endpoint or on a request or test step's Endpoint tab - the YAML file holds only a reference id, while the encrypted value lives in a `secrets.properties` file local to each machine. The reference ids travel with the request, test case and environment YAML files, and stay the same across the team. After checking out a workspace that contains secrets, a teammate sees blank values for those secrets on the ATB UI, and populates the values manually there - each value is then encrypted and stored in their own local `secrets.properties` file.

Recommended: share each ATB workspace as a separate VCS (like Git) repo.

Sharing a workspace through a Git repo also enables running its tests in a CI/CD pipeline, which checks out the same repo. Refer to [Integration Unit Testing Automation in CI/CD Pipeline](/docs/en/integration-unit-testing-automation-in-cicd-pipeline).