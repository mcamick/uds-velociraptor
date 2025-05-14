You can use `CODEOWNERS` in conjunction with GitLab's [merge request approvals](http://docs.gitlab.com/user/project/merge_requests/approvals/) feature to ensure that only authorized individuals can approve changes to specific files or directories within your project.

The current `CODEOWNERS` file that exists, is to denote who owns this `mission-app-template`. If you are creating a new project from this template, you will need to replace this file using the below example, then modify it to fit your needs. After updating the `CODEOWNERS` file to your liking, you can delete this file. 

The `CODEOWNERS` file should follow the below format. Refer to GitLab's [official documentation](https://docs.gitlab.com/user/project/codeowners/) for more information.

```md
[Development Org][2] @project-blue/your-org
/.gitlab/CODEOWNERS
/src/

[Defense Unicorns][2] @project-blue/defense-unicorns
/.gitlab/CODEOWNERS
/.gitlab-ci.yml
/uds/
```
