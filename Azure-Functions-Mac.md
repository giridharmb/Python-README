# Azure Functions Python Project Guide for macOS
[![Azure Functions](https://img.shields.io/badge/Azure-Functions-blue)](https://azure.microsoft.com/services/functions/)
[![Python](https://img.shields.io/badge/Python-3.9+-yellow)](https://www.python.org/)
[![macOS](https://img.shields.io/badge/macOS-Compatible-lightgrey)](https://www.apple.com/macos/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A comprehensive guide for creating, developing, and deploying Python-based Azure Functions on macOS.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Setup](#project-setup)
- [Local Development](#local-development)
- [Deployment](#deployment)
- [Common Issues](#common-issues)
- [Best Practices](#best-practices)

## Prerequisites

Before you begin, ensure you have the following installed on your Mac:

1. Install Homebrew (if not already installed):
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Install Python 3.9 or higher:
```bash
brew install python@3.9
```

3. Install Node.js (required for Azure Functions Core Tools):
```bash
brew install node
```

4. Install Azure CLI:
```bash
brew install azure-cli
```

5. Install Azure Functions Core Tools:
```bash
brew tap azure/functions
brew install azure-functions-core-tools@4
```

Verify installations:
```bash
# Check Python version
python3 --version

# Check Node.js version
node --version

# Check Azure CLI version
az --version

# Check Azure Functions Core Tools version
func --version
```

## Installation

1. Clone your repository (or create a new project):
```bash
git clone https://github.com/yourusername/azure-functions-python
cd azure-functions-python
```

2. Create and activate a virtual environment:
```bash
# Create virtual environment
python3 -m venv .venv

# Activate virtual environment
source .venv/bin/activate
```

3. Install required packages:
```bash
pip3 install -r requirements.txt
```

## Project Setup

1. Initialize a new function project:
```bash
func init --python

# Select Python when prompted for worker runtime
```

2. Create a new function:
```bash
func new --name HttpExample --template "HTTP trigger"
```

3. Create requirements.txt:
```bash
echo "azure-functions" > requirements.txt
```

4. Create local.settings.json:
```bash
{
    "IsEncrypted": false,
    "Values": {
        "FUNCTIONS_WORKER_RUNTIME": "python",
        "AzureWebJobsStorage": "UseDevelopmentStorage=true"
    }
}
```

## Local Development

1. Start the local development server:
```bash
func start
```

2. Test HTTP trigger function (using Terminal):
```bash
curl "http://localhost:7071/api/HttpExample?name=YourName"
```

### VS Code Setup for macOS

1. Install VS Code extensions:
   - Python extension
   - Azure Functions extension
   - Azure Account extension

2. Configure VS Code settings (Command+,):
```json
{
    "azureFunctions.pythonVenv": ".venv",
    "python.pythonPath": ".venv/bin/python",
    "python.terminal.activateEnvironment": true
}
```

## Deployment

1. Login to Azure:
```bash
az login
```

2. Create Azure resources:
```bash
# Set variables
export RESOURCE_GROUP="my-function-rg"
export STORAGE_ACCOUNT="myfuncstore$(openssl rand -hex 4)"
export FUNCTION_APP="my-function-app-$(openssl rand -hex 4)"
export LOCATION="eastus"

# Create resource group
az group create --name $RESOURCE_GROUP --location $LOCATION

# Create storage account
az storage account create \
    --name $STORAGE_ACCOUNT \
    --location $LOCATION \
    --resource-group $RESOURCE_GROUP \
    --sku Standard_LRS

# Create function app
az functionapp create \
    --resource-group $RESOURCE_GROUP \
    --consumption-plan-location $LOCATION \
    --runtime python \
    --runtime-version 3.9 \
    --functions-version 4 \
    --name $FUNCTION_APP \
    --os-type linux \
    --storage-account $STORAGE_ACCOUNT
```

3. Deploy your function:
```bash
func azure functionapp publish $FUNCTION_APP
```

## Common Issues

### 1. Python Version Conflicts
If you have multiple Python versions installed:
```bash
# Use pyenv to manage Python versions
brew install pyenv
pyenv install 3.9.7
pyenv global 3.9.7
```

### 2. Permission Issues
If you encounter permission errors:
```bash
# Fix permissions for Homebrew
sudo chown -R $(whoami) $(brew --prefix)/*

# Fix permissions for Python
sudo chmod -R 755 /Library/Python/3.9
```

### 3. Core Tools Issues
If Azure Functions Core Tools isn't working:
```bash
# Reinstall Core Tools
brew uninstall azure-functions-core-tools@4
brew cleanup
brew install azure-functions-core-tools@4
```

### 4. Virtual Environment Issues
If virtual environment isn't activating:
```bash
# Recreate virtual environment
deactivate
rm -rf .venv
python3 -m venv .venv
source .venv/bin/activate
```

## Best Practices for macOS

1. **Managing Python Versions:**
```bash
# Install pyenv for better Python version management
brew install pyenv
pyenv install 3.9.7
pyenv global 3.9.7

# Add to ~/.zshrc or ~/.bash_profile
eval "$(pyenv init -)"
```

2. **Environment Management:**
```bash
# Create alias for activating virtual environment
echo 'alias venv="source .venv/bin/activate"' >> ~/.zshrc
source ~/.zshrc
```

3. **VS Code Integration:**
```json
// settings.json
{
    "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
    "python.terminal.activateEnvironment": true,
    "files.exclude": {
        "**/.venv": false,
        "**/__pycache__": true,
        "**/.pytest_cache": true
    }
}
```

4. **Using Terminal Profiles:**
Create a dedicated Terminal profile for Azure Functions development:
```bash
# Add to ~/.zshrc
function azfunc() {
    source .venv/bin/activate
    export AZURE_FUNCTIONS_ENVIRONMENT=Development
    echo "Azure Functions development environment activated"
}
```

## Project Structure for macOS
```
azure-functions-python/
├── .venv/                      # Virtual environment
├── .vscode/                    # VS Code settings
│   ├── launch.json            # Debug configuration
│   └── settings.json          # VS Code settings
├── HttpExample/               # HTTP trigger function
│   ├── __init__.py           # Function code
│   └── function.json         # Function configuration
├── tests/                    # Unit tests
├── .gitignore
├── host.json                 # Host configuration
├── local.settings.json       # Local settings
├── requirements.txt          # Python dependencies
└── README.md                # This file
```

## Testing

Run tests using pytest:
```bash
# Install pytest
pip3 install pytest pytest-asyncio

# Run tests
python3 -m pytest tests/ -v
```

## Monitoring

View logs in real-time:
```bash
# Using Azure CLI
az webapp log tail --name $FUNCTION_APP --resource-group $RESOURCE_GROUP

# Using Functions Core Tools
func azure functionapp logstream $FUNCTION_APP
```

## Cleanup

Remove all resources:
```bash
az group delete --name $RESOURCE_GROUP --yes
```

---

## Additional macOS Tips

1. **Terminal Alternatives:**
   - Install iTerm2: `brew install --cask iterm2`
   - Configure for better Python development

2. **Development Tools:**
   - Install Postman: `brew install --cask postman`
   - Install Docker: `brew install --cask docker`

3. **Useful Aliases:**
Add to `~/.zshrc`:
```bash
alias azf='func start'
alias azd='func azure functionapp publish'
alias azl='az webapp log tail'
```
---
