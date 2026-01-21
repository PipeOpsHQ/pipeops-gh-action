# PipeOps Deploy GitHub Action

A GitHub Action for deploying applications to [PipeOps](https://pipeops.io) platform using the official PipeOps CLI.

## Features

- 🚀 **Easy Deployment**: Deploy your applications to PipeOps with a single action
- 🔐 **Secure Authentication**: Token-based authentication using GitHub Secrets
- 📦 **CLI Integration**: Uses the official PipeOps CLI for reliable deployments
- 📊 **Structured Outputs**: Provides deployment ID, URL, and status as action outputs
- 🎯 **Flexible Configuration**: Support for multiple environments, branches, and pipelines
- 🔄 **JSON Output**: Parses CLI JSON output for structured data extraction

## Usage

### Basic Deployment

```yaml
name: Deploy to PipeOps

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to PipeOps
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: 'your-project-id'
          environment: 'production'
          branch: ${{ github.ref_name }}
```

### Multi-Environment Deployment

```yaml
name: Deploy to PipeOps

on:
  push:
    branches: [ main, develop ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Staging
        if: github.ref == 'refs/heads/develop'
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: 'your-project-id'
          environment: 'staging'
          branch: develop

      - name: Deploy to Production
        if: github.ref == 'refs/heads/main'
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: 'your-project-id'
          environment: 'production'
          branch: main
```

### Deployment with Pipeline

```yaml
name: Deploy to PipeOps

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to PipeOps
        id: deploy
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: 'your-project-id'
          pipeline: 'my-pipeline-id'
          environment: 'production'

      - name: Display deployment info
        run: |
          echo "Deployment ID: ${{ steps.deploy.outputs.deployment-id }}"
          echo "Deployment URL: ${{ steps.deploy.outputs.deployment-url }}"
          echo "Deployment Status: ${{ steps.deploy.outputs.deployment-status }}"
```

### Conditional Deployment

```yaml
name: Deploy to PipeOps

on:
  push:
    branches: [ main, develop ]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to PipeOps
        if: github.event_name == 'push' || github.event.inputs.deploy == 'true'
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: ${{ secrets.PIPEOPS_PROJECT_ID }}
          environment: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
          branch: ${{ github.ref_name }}
```

### Deployment from Subdirectory

```yaml
name: Deploy to PipeOps

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy to PipeOps
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: 'your-project-id'
          working-directory: './frontend'
          environment: 'production'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `pipeops-token` | PipeOps API token/authentication token | Yes | - |
| `project-id` | PipeOps project ID | Yes | - |
| `environment` | Deployment environment | No | `production` |
| `branch` | Git branch to deploy | No | Current branch (`${{ github.ref_name }}`) |
| `pipeline` | Pipeline name or ID if using `pipeops deploy pipeline` | No | - |
| `working-directory` | Directory to run commands from | No | Repository root |
| `cli-version` | PipeOps CLI version to install | No | `latest` |

## Outputs

| Output | Description |
|-------|-------------|
| `deployment-id` | ID of the created deployment |
| `deployment-url` | URL of the deployed application |
| `deployment-status` | Status of the deployment |

## Prerequisites

1. **PipeOps Account**: You need a PipeOps account. Sign up at [pipeops.io](https://pipeops.io)

2. **API Token**: Generate an API token from your PipeOps dashboard:
   - Go to your PipeOps dashboard
   - Navigate to Settings → API Tokens
   - Create a new token
   - Copy the token value

3. **GitHub Secret**: Add your PipeOps token as a GitHub Secret:
   - Go to your repository Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `PIPEOPS_TOKEN`
   - Value: Your PipeOps API token
   - Click "Add secret"

4. **Project ID**: Get your project ID from the PipeOps dashboard:
   - Navigate to your project in PipeOps
   - Copy the project ID from the project settings or URL

## Requirements

- GitHub Actions runner with bash support (Linux/Windows)
- Network access to:
  - GitHub (for CLI installation script)
  - PipeOps API/services (for authentication and deployment)

The action automatically installs:
- `jq` (for JSON parsing)
- PipeOps CLI (via official install script)

## Error Handling

The action handles various error scenarios:

- **Missing Required Inputs**: Validates that `pipeops-token` and `project-id` are provided
- **CLI Installation Failures**: Provides clear error messages if CLI installation fails
- **Authentication Errors**: Handles invalid or expired tokens with helpful error messages
- **Deployment Failures**: Captures and displays deployment errors with exit codes
- **Network Issues**: Reports connectivity problems clearly

## Troubleshooting

### Authentication Failed

If you see authentication errors:

1. Verify your `PIPEOPS_TOKEN` secret is correctly set in GitHub
2. Check that the token hasn't expired
3. Ensure the token has the necessary permissions for deployment

### CLI Installation Failed

If CLI installation fails:

1. Check network connectivity to GitHub
2. Verify the install script URL is accessible
3. Check GitHub Actions runner logs for detailed error messages

### Deployment Failed

If deployment fails:

1. Verify your project ID is correct
2. Check that the specified environment exists in your PipeOps project
3. Ensure the branch name is correct
4. Review the deployment output in the action logs

### Outputs Not Set

If outputs are not being set:

1. Check that the deployment was successful
2. Verify the CLI is outputting JSON format (using `--json` flag)
3. Review the action logs to see the raw CLI output

## Examples

### Complete Workflow with Build and Deploy

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to PipeOps
        id: deploy
        uses: PipeOpsHQ/pipeops-action@v1
        with:
          pipeops-token: ${{ secrets.PIPEOPS_TOKEN }}
          project-id: ${{ secrets.PIPEOPS_PROJECT_ID }}
          environment: 'production'
          branch: ${{ github.ref_name }}

      - name: Notify deployment
        run: |
          echo "✅ Deployment completed!"
          echo "Deployment ID: ${{ steps.deploy.outputs.deployment-id }}"
          echo "URL: ${{ steps.deploy.outputs.deployment-url }}"
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License.

## Support

- **Documentation**: [PipeOps Docs](https://docs.pipeops.io/)
- **CLI Repository**: [PipeOps CLI](https://github.com/PipeOpsHQ/pipeops-cli)
- **Issues**: [GitHub Issues](https://github.com/PipeOpsHQ/pipeops-action/issues)

## Related Resources

- [PipeOps Platform](https://pipeops.io)
- [PipeOps CLI](https://github.com/PipeOpsHQ/pipeops-cli)
- [PipeOps Go SDK](https://github.com/PipeOpsHQ/pipeops-go-sdk)
- [PipeOps Documentation](https://docs.pipeops.io/)
# pipeops-gh-action
