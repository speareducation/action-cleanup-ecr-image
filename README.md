# Action: Cleanup ECR Image for Branch

## Introduction

This action provides the ability to remove a docker tag from Amazon ECR based on the
git tag specified.

### Assumptions

This plugin assumes that your releases are tagged in Git as follows:

```
releases/<environment>/<release-name>
```

As an example, the following releases are valid, and translate as follows:

| Git branch name                   | ECR Tag name             |
|-----------------------------------|--------------------------|
| releases/sandbox/abc-123          | sandbox-abc-123          |
| releases/production/2021.03.16.01 | production-2021.03.16.01 |
| releases/staging/2.1.2            | staging-2.1.2            |

### Example

In your workflow, create a cleanup.yml that contains the following:

```yml
name: ECR Cleanup

on:
  delete:
    branches:
      - releases/sandbox/*
      - 
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: "ECR Cleanup"
        uses: speareducation/action-cleanup-ecr-image@main
        with:
            aws-access-key-id: <your access key>
            aws-secret-access-key: <your secret key>
            aws-region: <your aws region, e.g. us-east-1>
            aws-session-token: <optional, omit if empty>
            git-ref: ${{ github.event.ref }}
```
