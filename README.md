# Azure Resource Deployment with Bicep

This repository contains a sample project that demonstrates how to deploy resources to Azure using Azure Bicep. Bicep is a domain-specific language (DSL) that simplifies the authoring of ARM templates.

## Prerequisites

Before you begin, ensure you have:

- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) installed.
- [Bicep CLI](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/install) installed.
- An [Azure account](https://azure.microsoft.com/free/) (if you don't have one).

## Setup

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/azure-bicep-deployment.git
   cd azure-bicep-deployment
   ```

2. **Login to Azure**:
   ```bash
   az login
   ```

3. **Set the Subscription**:
   Set the subscription where you want to deploy the resources:
   ```bash
   az account set --subscription "your-subscription-id"
   ```

## Deployment

1. **Create a Resource Group**:
   Replace `your-resource-group` and `your-location` with your desired resource group name and Azure region:
   ```bash
   az group create --name your-resource-group --location your-location
   ```

2. **Compile the Bicep Template**:
   Convert the Bicep template to an ARM JSON template:
   ```bash
   az bicep build main.bicep
   ```

   This will generate a `main.json` file in the same directory.

3. **Deploy the Bicep Template**:
   Deploy the compiled ARM template (`main.json`) to the created resource group:
   ```bash
   az deployment group create --resource-group your-resource-group --template-file main.json
   ```

4. **Verify Deployment**:
   Verify the resources have been created successfully:
   ```bash
   az resource list --resource-group your-resource-group
   ```

## Files

- `main.bicep`: The main Bicep template file that defines the Azure resources to be deployed.
- `main.json`: The generated ARM template file compiled from the Bicep template.

## Example

Here is the content of the `main.bicep` file:

```bicep
param location string = resourceGroup().location
param storageAccountName string = 'bicep${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-04-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

This Bicep template creates a Storage Account with a unique name in the specified location.

## Helpful resource

- [Azure Bicep Documentation](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/)

---

Feel free to customize this README to better suit your project's specifics. Happy deploying!
