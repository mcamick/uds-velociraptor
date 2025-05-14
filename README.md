# Mission App Template

This mission app template is a starting point for creating a new mission application for the Project Blue Mission Environment (PBME). It includes a basic structure for creating a UDS package and a set of tasks to help you get started.

After creating a project from this template, follow [Making it your own](#make-it-your-own) documentation in order to customize the template for a new application package.

## Make it your own

1. Replace some common placeholders

| value                                 | replace_with                | example                                                |
| ------------------------------------- | --------------------------- | ------------------------------------------------------ |
| `#TEMPLATE_APPLICATION_NAME#`         | application name            | nginx, mattermost, beast-core, etc...                |
| `#TEMPLATE_APPLICATION_DISPLAY_NAME#` | application name for humans | NGINX, Mattermost, Beast Core, etc...                 |
| `#TEMPLATE_CHART_REPO#`               | chart repository URL        | `https://charts.jetstack.io/`                          |
| `#UDS_PACKAGE_REPO#`                  | package repository URL      | `https://github.com/defenseunicorns/uds-package-nginx` |

2. Review, determine your need, and update

<!-- TODO: write guidance on how to customize the template for a new application package -->

3. Almost there...
   - `mv README-template.md README.md`
   - Follow the `CODEOWNERS-template.md` to update your `CODEOWNERS` file.

You are ready to start integrating (and testing with CI) your application for PBME!
