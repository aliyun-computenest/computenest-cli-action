# ComputeNest CLI GitHub Action

This GitHub workflow relies on the ComputeNest CLI tool developed by Alibaba Cloud. ComputeNest CLI automatically detects your code repository and converts it into a deployable service on Alibaba Cloud in the form of a Docker Compose deployment, Dockerfile build deployment, script deployment, or BuildPacks deployment. With this service, you can deploy and use your application with a single click and also share it with users for deployment.


## Key Features

- **GitHub Integration:** By default, merging into the "main" branch automatically triggers the ComputeNest service update process.
- **Automated Deployment Artifacts:** The system automatically detects whether a rebuild of deployment artifacts is necessary.

## Inputs

- `AK_ID` (**required**): Access Key ID.
- `AK_SECRET` (**required**): Access Key Secret.
- `SERVICE_NAME` (**required**): The name of the service.
- `REGION_ID` (**optional**): Region ID. Only `ap-southeast-1` and `cn-hangzhou` are allowed. Default is `cn-hangzhou`.

## Outputs

An updated service on Alibaba Cloud

## Secrets Management

Secrets such as `AK_ID`, `AK_SECRET`, and `SERVICE_NAME` should be stored in the default environment within your GitHub repository settings. This ensures that your sensitive information is kept secure and is only accessible to those with appropriate permissions.

```yaml
with:
  AK_ID: ${{ secrets.AK_ID }}
  AK_SECRET: ${{ secrets.AK_SECRET }}
  SERVICE_NAME: ${{ secrets.SERVICE_NAME }}
  REGION_ID: 'cn-hangzhou'
```

## Example

```yaml
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    environment: default  # 确保 'default' 环境中已设置 Secrets
    steps:
      - uses: actions/checkout@v4

      - name: ComputeNest Build-Service Deployment
        uses: aliyun-computenest/computenest-cli-action@version
        with:
          ak_id: ${{ secrets.AK_ID }}
          ak_secret: ${{ secrets.AK_SECRET }}
          service_name: "YourServiceName"  # 替换为你的服务名称
          region_id: "cn-hangzhou"         # 可选，默认值已在 Action 中设置

      - name: Run a multi-line script
        run: |
          echo "Add other actions to build,"
          echo "test, and deploy your project."
