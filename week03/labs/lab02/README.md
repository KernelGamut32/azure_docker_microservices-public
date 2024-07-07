# Lab 02 - [Microservice Backend in Azure](https://learn.microsoft.com/en-us/azure/container-apps/tutorial-code-to-cloud?tabs=bash%2Ccsharp&pivots=acr-remote)

**NOTE: Use an A Cloud Guru (ACG) Azure Playground for this lab**

1. Login to `acloudguru.com`, navigate to the `Playground` tab, and click the `Start Azure Sandbox` button
1. Right click the `Open Sandbox` button and select to open in an Incognito or InPrivate window (depending on the browser you're using)
1. Use the provided credentials to login to the Azure portal
1. Launch `Cloud Shell` in the Azure portal
    - Use `Bash`
    - Click `No Storage Account Required`
    - For `Subscription` select the available option
    - Click `Apply`
    - In the `Cloud Shell` terminal click the `Settings` dropdown and select `Go to Classic version`; this will allow you to work with source code in the shell using `code .`
1. Configure environment variables as follows:

```
export RESOURCE_GROUP=$(az group list --query [*].name --output tsv)
export LOCATION=$(az group list --query [*].location --output tsv)
export ACR_NAME=acaalbums$RANDOM
export API_NAME=albumsapi
```

6. Clone the repository for the language of your choice into `Cloud Shell` and navigate to the project folder using `cd code-to-cloud/src`; use `Azure-Samples` instead of `$GITHUB_USERNAME` in the clone statement
1. Do not create a new resource group - use the one provided in the sandbox
1. Create a new ACR using `az acr create --resource-group $RESOURCE_GROUP --location $LOCATION --sku Basic --name $ACR_NAME --admin-enabled true`
1. Execute `az acr list --query [*].name` (or review in Azure Portal) to confirm creation
1. From the `code-to-cloud/src` folder, use `az acr build --registry $ACR_NAME --image $API_NAME .` to build the image for the application
1. Navigate to the `Repositories` area of your ACR to confirm creation of the image
1. Instead of using Container Apps (which are not enabled in the ACG sandboxes), we're going to deploy as an Azure App Service using our ACR image as deployment source
    - Search for `App Services` and click `Create > Web App`
    - Use the available Resource Group
    - For `Web App name`, use something unique like `albumsapi-<initials><year><month><day>`
    - Choose `Container` for the `Publish` source
    - For region, use the same region defined on the resource group
    - For `Pricing plan`, choose `Basic B1`
    - Click the `Container` tab
    - Select `Azure Container Registry` as the `Image Source`
    - Select your ACR and your previously built image and tag
    - Click `Review + create` and click `Create`
    - On completion, click `Go to resource`
    - Try hitting the API endpoint using `https://<Default domain for API service>/albums` with a GET in a browser or in POSTMan (it may take a couple of minutes for the site to come up in Azure)
1. If moving to lab 03, leave the API resources in place and pick up from the lab 03 README