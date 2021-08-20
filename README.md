## Amazon App Mesh Virtual Router Action for GitHub Actions

[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

Creates an Amazon App Mesh Virtual Router

**Table of Contents**

<!-- toc -->

- [Amazon App Mesh Virtual Router Action for GitHub Actions](#amazon-app-mesh-virtual-router-action-for-github-actions)
- [Usage](#usage)
  - [Creating the resource](#creating-the-resource)
  - [Deleting the resource](#deleting-the-resource)
- [Credentials and Region](#credentials-and-region)
  - [Deleting the resource](#deleting-the-resource-1)
- [Permissions](#permissions)
- [Troubleshooting](#troubleshooting)

<!-- tocstop -->

## Usage

### Creating the resource

```yaml
- name: App Mesh Virtual Router
  uses: scribd/amazon-appmesh-virtual-router@master
  with:
    name: my-virtual-router
    mesh-name: my-mesh-name
    spec: |
      {
        "listeners": [
          {
            "portMapping": {
              "port": 80,
              "protocol": "http"
            }
          }
        ]
      }
    tags: |
      [
        {
          "key": "tags-must",
          "value": "be-passed-in-as-a-json-string"
        },
        {
          "key": "this-is-because",
          "value": "GitHub Actions translates these values into Environment Variables"
        }
      ]
```

### Deleting the resource

Deleting the resource requires `name`, `mesh-name`, and `action` to be specified as `delete`. `mesh-owner` may be supplied if required. Any additional parameters will be ignored.

```yaml
- name: App Mesh Virtual Router
  uses: scribd/amazon-appmesh-virtual-router@master
  with:
    name: my-virtual-router
    mesh-name: my-mesh-name
    action: delete
```

See [action.yml](action.yml) for the full documentation for this action's inputs and outputs.


## Credentials and Region

This action relies on the [default behavior of the AWS SDK for Javascript](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-node.html) to determine AWS credentials and region.
Use [the `aws-actions/configure-aws-credentials` action](https://github.com/aws-actions/configure-aws-credentials) to configure the GitHub Actions environment with environment variables containing AWS credentials and your desired region.

We recommend following [Amazon IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) for the AWS credentials used in GitHub Actions workflows, including:
* Do not store credentials in your repository's code.  You may use [GitHub Actions secrets](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) to store credentials and redact credentials from GitHub Actions workflow logs.
### Deleting the resource
* [Create an individual IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users) with an access key for use in GitHub Actions workflows, preferably one per repository. Do not use the AWS account root user access key.
* [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) to the credentials used in GitHub Actions workflows.  Grant only the permissions required to perform the actions in your GitHub Actions workflows.  See the Permissions section below for the permissions required by this action.
* [Rotate the credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#rotate-credentials) used in GitHub Actions workflows regularly.
* [Monitor the activity](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#keep-a-log) of the credentials used in GitHub Actions workflows.

## Permissions

This action requires the following minimum set of permissions:

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"DeployRouter",
      "Effect":"Allow",
      "Action":[
        "appmesh:DescribeVirtualRouter",
        "appmesh:DeleteRouter",
        "appmesh:CreateVirtualRouter"
      ],
      "Resource":[
        "arn:aws:appmesh:us-east-1:1234567890:mesh/my-mesh-name/virtualRouter/my-virtual-router"
      ]
    }
  ]
}
```


## Troubleshooting

This action emits debug logs to help troubleshoot deployment failures.  To see the debug logs, create a secret named `ACTIONS_STEP_DEBUG` with value `true` in your repository.

To run this action from your workstation, you should supply your environment variables with the `env` command, since BASH will (correctly) refuse to set environment variables with dashes in them.

If submitting an issue, please include output styled after the following example, it'll help greatly! Don't forget to redact any sensitive data from your submission.

See this example:

```bash
env 'ACTIONS_STEP_DEBUG=true' "GITHUB_WORKSPACE=$(pwd)" 'AWS_REGION=us-east-1' 'INPUT_SPEC={"a": "b", "c": [{"d": "e"}]}' 'INPUT_MESH-NAME=example-mesh' 'INPUT_NAME=my-service' 'INPUT_TAGS=[{"Key": "hello", "Value": "world"}]' node  index.js
```

# Development

Releases are cut using [semantic-release](https://github.com/semantic-release/semantic-release).

Please write commit messages following [Angular commit guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)