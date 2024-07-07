# Lab 03 - [Microservice Frontend in Azure](https://learn.microsoft.com/en-us/azure/container-apps/communicate-between-microservices?tabs=bash&pivots=acr-remote)

**NOTE: Use an A Cloud Guru (ACG) Azure Playground for this lab**

1. This lab is intended to walk through the process of creating a containerized front-end microservice that communicates with a containerized back-end microservice running in Azure - the recommended sequence is lab 02 followed by lab 03 running in the same ACG sandbox
1. Confirm that the following environment variables exist:

```
export RESOURCE_GROUP=$(az group list --query [*].name --output tsv)
export LOCATION=$(az group list --query [*].location --output tsv)
export ACR_NAME=acaalbums$RANDOM
export API_NAME=albumsapi
export FRONTEND_NAME=albumsui
```

3. In `Cloud Shell`, navigate to the home folder using `cd ~`
1. Clone the front-end repository using `git clone https://github.com/Azure-Samples/containerapps-albumui.git code-to-cloud-ui`
1. Navigate to the project folder using `cd code-to-cloud-ui/src`
1. From the `code-to-cloud-ui/src` folder, use `az acr build --registry $ACR_NAME --image $FRONTEND_NAME .` to build the image for the application
1. Navigate to the `Repositories` area of your ACR to confirm creation of the image
1. Instead of using Container Apps (which are not enabled in the ACG sandboxes), we're going to deploy as an Azure App Service using our ACR image as deployment source
    - Search for `App Services` and click `Create > Web App`
    - Use the available Resource Group
    - For `Web App name`, use something unique like `albumsui-<initials><year><month><day>`
    - Choose `Container` for the `Publish` source
    - For region, use the same region defined on the resource group
    - For `Pricing plan`, choose `Basic B1`
    - Click the `Container` tab
    - Select `Azure Container Registry` as the `Image Source`
    - Select your ACR and your previously built image and tag
    - Click `Review + create` and click `Create`
    - On completion, click `Go to resource`
    - Under `Settings > Environment variables`, add a new setting with the name `API_BASE_URL` and the value of the `Default domain` for your API app service
    - Try hitting the UI endpoint using `https://<Default domain for UI service>` and confirm display of album details
