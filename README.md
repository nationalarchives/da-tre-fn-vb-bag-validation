# tre-fn-vb-bag-validation

## GitHub Actions

| Action | Description |
| --- | --- |
| [`.github/workflows/deploy-to-ecr.yml`](.github/workflows/deploy-to-ecr.yml) | Gets next git version tag<br>Builds and deploys Docker image to ECR<br>If successful, tags git with new version |

## GitHub Action Secrets

Secret for main AWS authentication (using OpenID Connect):

| Name                         | Description                                          |
| ---------------------------- | ---------------------------------------------------- |
| AWS_OPEN_ID_CONNECT_ROLE_ARN | ARN of AWS role used to authenticate GitHub with AWS |

Other secrets:

| Name                                | Description                                                              |
| ----------------------------------- | ------------------------------------------------------------------------ |
| AWS_CODEARTIFACT_REPOSITORY_NAME    | Name of AWS CodeArtifact repository to log in to for additional packages |
| AWS_CODEARTIFACT_REPOSITORY_DOMAIN  | Name of AWS CodeArtifact repository's domain                             |
| AWS_CODEARTIFACT_REPOSITORY_ACCOUNT | The AWS account ID that owns the CodeArtifact                            |
| AWS_REGION                          | The AWS region to use for CodeArtifact and ECR                           |

## Development Environment Setup Steps

```bash
# Create a Python virtual environment
python3 -m venv .venv
. ./.venv/bin/activate

# Log in to AWS codeartifact
aws codeartifact login \
  --tool pip \
  --repository "${AWS_CODEARTIFACT_REPOSITORY_NAME}" \
  --domain "${AWS_CODEARTIFACT_REPOSITORY_DOMAIN}" \
  --domain-owner "${AWS_CODEARTIFACT_REPOSITORY_ACCOUNT}" \
  --region "${AWS_CODEARTIFACT_REPOSITORY_REGION}" \
  --profile "${AWS_PROFILE}"

# Install required libraries from AWS codeartifact
pip install \
  --require-virtualenv \
  --requirement requirements_aws.txt
```
