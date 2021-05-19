# AWS ECR Push and ECS Deploy

## Example Usage

```yaml
on:
  push:
    branches:
      - master

name: Push image to ECR and force new ECS deploy

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-west-2

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v1

      - name: Build and redeploy
        uses: jaroldwong/ecr-push-and-ecs-deploy@v1.1
        with:
          ecr-registry: ${{ steps.login-ecr.outputs.registry }}
          ecr-repository: 'Repository name'
          ecs-cluster: 'ECS Cluster name'
          ecs-service: 'Service name'
```
