# Mission App Template

This mission app template is a starting point for creating a new mission application for the Project Blue Mission Environment (PBME). It includes a basic structure for creating a UDS package and a set of tasks to help you get started.

After creating a project from this template, follow the documentation below in order to customize the template for a new application package.

## Make it your own

1. Replace some common placeholders

| value         | replace_with                | example                               |
| ------------- | --------------------------- | ------------------------------------- |
| `hello-world` | application name            | nginx, mattermost, beast-core, etc... |
| `Hello World` | application name for humans | NGINX, Mattermost, Beast Core, etc... |
| `PBME Mission App Template` | application description | `The greatest full-stack app PBME has ever seen` |
| `project-blue/templates/mission-app-template/images/` | container registry repository | `project-blue/narwhal-demo/prance/prance-app-1/images` |

2. Review, determine your need, and update

<!-- TODO: write guidance on how to customize the template for a new application package -->

3. Almost there...
   - `mv README-template.md README.md`
   - Follow the `CODEOWNERS-template.md` to update your `CODEOWNERS` file.

You are ready to start integrating (and testing with CI) your application for PBME!
