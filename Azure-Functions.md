# Azure Functions Python Project Guide
[![Azure Functions](https://img.shields.io/badge/Azure-Functions-blue)](https://azure.microsoft.com/services/functions/)
[![Python](https://img.shields.io/badge/Python-3.9+-yellow)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A comprehensive guide for creating, developing, and deploying Python-based Azure Functions. This project provides examples and best practices for building serverless applications using Azure Functions with Python.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Local Development](#local-development)
- [Deployment](#deployment)
- [Function Types](#function-types)
- [Configuration](#configuration)
- [Testing](#testing)
- [Monitoring](#monitoring)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following installed:

- Python 3.9 or higher
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
- [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local)
- An active Azure subscription
- Git (optional, for version control)

```bash
# Check Python version
python --version

# Check Azure CLI version
az --version

# Check Azure Functions Core Tools version
func --version
```

## Installation

1. Clone this repository (or create a new project):
```bash
git clone https://github.com/yourusername/azure-functions-python
cd azure-functions-python
```

2. Create and activate a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install required packages:
```bash
pip install -r requirements.txt
```

## Project Structure

```
azure-functions-python/
├── .venv/                      # Virtual environment
├── HttpExample/                # HTTP trigger function
│   ├── __init__.py            # Function code
│   └── function.json          # Function configuration
├── TimerExample/              # Timer trigger function
│   ├── __init__.py
│   └── function.json
├── tests/                     # Unit tests
│   └── test_functions.py
├── .gitignore
├── host.json                  # Host configuration
├── local.settings.json        # Local settings (git-ignored)
├── requirements.txt           # Python dependencies
└── README.md                 # This file
```

## Local Development

1. Start the local development server:
```bash
func start
```

2. Test HTTP trigger function locally:
```bash
curl "http://localhost:7071/api/HttpExample?name=YourName"
```

3. Debug using VS Code:
   - Install Python extension
   - Set breakpoints
   - Press F5 to start debugging

### Local Settings

Create a `local.settings.json` file:
```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true"
  }
}
```

## Deployment

1. Login to Azure:
```bash
az login
```

2. Create required Azure resources:
```bash
# Set variables
RESOURCE_GROUP="my-function-rg"
STORAGE_ACCOUNT="myfuncstore$RANDOM"
FUNCTION_APP="my-function-app-$RANDOM"
LOCATION="eastus"

# Create resources
az group create --name $RESOURCE_GROUP --location $LOCATION
az storage account create --name $STORAGE_ACCOUNT --location $LOCATION --resource-group $RESOURCE_GROUP --sku Standard_LRS
az functionapp create --resource-group $RESOURCE_GROUP --consumption-plan-location $LOCATION --runtime python --runtime-version 3.9 --functions-version 4 --name $FUNCTION_APP --os-type linux --storage-account $STORAGE_ACCOUNT
```

3. Deploy your function:
```bash
func azure functionapp publish $FUNCTION_APP
```

## Function Types

### HTTP Trigger
```python
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    name = req.params.get('name')
    return func.HttpResponse(f"Hello, {name}!")
```

### Timer Trigger
```python
import azure.functions as func

def main(timer: func.TimerRequest) -> None:
    # Runs on schedule defined in function.json
    logging.info('Timer trigger function ran')
```

### Blob Trigger
```python
import azure.functions as func

def main(myblob: func.InputStream):
    logging.info(f"Blob trigger processed blob: {myblob.name}")
```

## Configuration

### Application Settings
Set in Azure Portal or using Azure CLI:
```bash
az functionapp config appsettings set --name $FUNCTION_APP --resource-group $RESOURCE_GROUP --settings "MySetting=MyValue"
```

### Environment Variables
Access in code:
```python
import os
setting_value = os.environ["MySetting"]
```

## Testing

1. Install test dependencies:
```bash
pip install pytest pytest-asyncio
```

2. Run tests:
```bash
pytest tests/
```

Example test:
```python
import azure.functions as func
import pytest
from HttpExample import main

def test_http_trigger():
    request = func.HttpRequest(
        method='GET',
        body=None,
        url='/api/HttpExample',
        params={'name': 'Test'}
    )
    response = main(request)
    assert response.status_code == 200
```

## Monitoring

1. View logs in real-time:
```bash
az webapp log tail --name $FUNCTION_APP --resource-group $RESOURCE_GROUP
```

2. Application Insights:
- Enable in Azure Portal
- Add connection string to application settings
- View metrics, logs, and traces

## Troubleshooting

Common issues and solutions:

1. **Function not starting locally:**
   - Check Python version
   - Verify virtual environment is activated
   - Confirm all dependencies are installed

2. **Deployment failures:**
   - Check deployment logs
   - Verify Azure credentials
   - Ensure resources are properly configured

3. **Runtime errors:**
   - Check function logs
   - Verify environment variables
   - Review application insights

## Best Practices

1. **Security:**
   - Use managed identities
   - Store secrets in Azure Key Vault
   - Implement proper authentication

2. **Performance:**
   - Optimize cold starts
   - Use async/await for I/O operations
   - Implement caching where appropriate

3. **Reliability:**
   - Implement proper error handling
   - Use retry patterns
   - Log appropriately

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.