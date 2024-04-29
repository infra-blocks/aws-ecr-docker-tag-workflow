# aws-ecr-docker-tag-workflow
[![Git Tag Semver From Label](https://github.com/infrastructure-blocks/aws-ecr-docker-tag-workflow/actions/workflows/git-tag-semver-from-label.yml/badge.svg)](https://github.com/infrastructure-blocks/aws-ecr-docker-tag-workflow/actions/workflows/git-tag-semver-from-label.yml)
[![Update From Template](https://github.com/infrastructure-blocks/aws-ecr-docker-tag-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infrastructure-blocks/aws-ecr-docker-tag-workflow/actions/workflows/update-from-template.yml)

<<<<<<< HEAD
This reusable workflow logs assumes an AWS role through GitHub OIDC, logs into AWS ECR and tags an existing
image.

At the time of this writing, only AWS ECR Public is supported.
=======
This repository is a template for creating reusable GitHub Actions Workflows. Go through the below checklist
upon instantiating this template:
- Remove the [trigger update from template workflow](.github/workflows/trigger-update-from-template.yml)
- Edit the content of [the placeholder](.github/workflows/workflow.yml) for your reusable workflow.
- Update the status badges:
    - Remove the `Trigger Update From Template` status badge.
    - Add the `Update From Template` status badge.
    - Rename the rest of the links to point to the right repository.
- Edit this document and update the relevant sections
- Prepare the [changelog](CHANGELOG.md) for the first version of the module that will be released.
>>>>>>> template/master

## Inputs

|    Name    | Required | Description                                                                                                                                                                                      |
|:----------:|:--------:|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  aws-role  |   true   | The AWS role to assume by the CI.                                                                                                                                                                |
| aws-region |   true   | The AWS region of the role.                                                                                                                                                                      |
|   image    |   true   | The docker image to tag. This can include the tag (repo:tag), the digest (repo@sha256:digest) or be just the repository. In the latter case, docker defaults to the image with the "latest" tag. |
|    skip    |  false   | A boolean indicating whether to skip the workflow. This is to workaround the required checks discrepancy when the workflow is skipped from the caller. It defaults to false.                     |
|    tags    |   true   | A stringified JSON array of tags to apply.                                                                                                                                                       |

## Secrets

N/A

## Outputs

|   Name    | Description                                   |
|:---------:|-----------------------------------------------|
| published | A stringified JSON array of images published. |

## Permissions

|  Scope   | Level | Reason                                                |
|:--------:|:-----:|-------------------------------------------------------|
| id-token | write | Required to authenticate with AWS through GitHub OIDC |

## Concurrency controls

N/A

## Timeouts

N/A

## Usage

```yaml
name: AWS ECR Docker Tag

on:
  push:
    tags:
      - "v**"

permissions:
  id-token: write

jobs:
  aws-ecr-docker-tag:
    uses: infrastructure-blocks/aws-ecr-docker-tag-workflow/.github/workflows/workflow.yml@v1
    with:
      aws-role: ${{ vars.AWS-ROLE }}
      aws-region: ${{ vars.AWS-REGION }}
      image: public.ecr.aws/<your-alias>/<your-repo>:<some-tag>
      tags: '["new-tag-1", "new-tag-2"]'
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
