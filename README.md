# aws-ecr-docker-tag-workflow

This reusable workflow logs assumes an AWS role through GitHub OIDC, logs into AWS ECR and tags an existing
image.

At the time of this writing, only AWS ECR Public is supported.

## Usage

```yaml
name: AWS ECR Docker Tag

on:
  push:
    tags:
      - "v**"

permissions:
  id-token: write # Required to get a token from GitHub OIDC provider

jobs:
  aws-ecr-docker-tag:
    uses: infrastructure-blocks/aws-ecr-docker-tag-workflow/.github/workflows/aws-ecr-docker-tag.yml@v1
    with:
      aws-role: ${{ vars.AWS-ROLE }}
      aws-region: ${{ vars.AWS-REGION }}
      # The image can be just the repo (which implies the "latest" tag), be provided with a tag (repo:tag)
      # or a digest (repo@sha256:digest)
      image: public.ecr.aws/<your-alias>/<your-repo>:<some-tag>
      tags: '["new-tag-1", "new-tag-2"]'
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
