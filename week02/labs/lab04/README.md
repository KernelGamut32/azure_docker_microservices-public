# Lab 04 - [Containerized Python with MongoDB](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-03)

**NOTE: Use an A Cloud Guru (ACG) Azure Playground for this lab**

1. Login to `acloudgur/home/sysadmin/source/repos/azure_docker_microservices/week02/labs/lab02u.com`, navigate to the `Playground` tab, and click the `Start Azure Sandbox` button
1. Right click the `Open Sandbox` button and select to open in an Incognito or InPrivate window (depending on the browser you're using)
1. Use the provided credentials to login to the Azure portal
1. Launch `Cloud Shell` in the Azure portal
    - Use `Bash`
    - Click `No Storage Account Required`
    - For `Subscription` select the available option
    - Click `Apply`
    - In the `Cloud Shell` terminal click the `Settings` dropdown and select `Go to Classic version`; this will allow you to work with source code in the shell using `code .`
1. Clone the sample app into `Cloud Shell` using `git clone https://github.com/Azure-Samples/msdocs-python-flask-container-web-app.git`
1. Change to the project root directory using `cd msdocs-python-flask-container-web-app`
1. Execute `code .` to open the project directory in the code editor; review the source files including the provided `Dockerfile`
1. Code the various environment variables in `Cloud Shell` that you'll need for creating the resources via the Azure CLI
    - Execute `az group list --query [*].name` (https://learn.microsoft.com/en-us/cli/azure/query-azure-cli) to get the name of the available resource group
    - Execute `az group list --query [*].location` to get the region
    - Execute `LOCATION=<location-from-previous-step>`
    - Execute `RESOURCE_GROUP_NAME=<resource-group-name-from-previous-step>`
    - Execute `ACCOUNT_NAME=<cosmos-db-account-name-of-your-choosing>`
    - Execute `REGISTRY_NAME=<acr-registry-name-of-your-choosing>`
    - Execute `APP_SERVICE_NAME=<app-service-name-of-your-choosing>`
1. Use [Set up MongoDB](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-02?tabs=sample-app-git-clone%2Cvscode-docker%2Cterminal-bash%2Cmongodb-azure#3-set-up-mongodb) to create a MongoDB in Cosmos DB (following the `Azure Cosmos DB for MongoDB` steps); skip the resource group creation step
1. Use [Create an Azure Container Registry](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-03?tabs=azure-cli#1-create-an-azure-container-registry) to create an Azure Container Registry (following the `Azure CLI` steps); skip the resource group creation step
    - For Step 2, include `--admin-enabled true` at the end of the `az acr create` statement
1. Use [Build an image in Azure Container Registry](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-03?tabs=azure-cli#2-build-an-image-in-azure-container-registry) to build an image from the project folder and host in ACR (following the `Azure CLI` steps)
1. Use [Create the web app](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-04?tabs=azure-cli#1-create-the-web-app) to create a new web app (following the `Azure portal` steps)
1. Skip the [Configure managed identity and webhook](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-04?tabs=azure-cli#2-configure-managed-identity-and-webhook) section; this section is used to add a managed identity to your web app and a web hook that can automatically pull and deploy an updated image when pushed to ACR. Unfortunately, the ACG sandboxes do not allow the assignment of roles.
    - If you receive an error about the pricing tier `Basic` not being allowed in the resource group, repeat the steps using the `Free` tier instead
1. Use [Configure connection to MongoDB](https://learn.microsoft.com/en-us/azure/developer/python/tutorial-containerize-deploy-python-web-app-azure-04?tabs=azure-cli#3-configure-connection-to-mongodb) to update application settings for connection to the Cosmos DB (following the `Azure CLI` steps)
    - Replace `$APP_SERVICE_NAME` with the name used for your
1. Browse and test the site when all steps are complete; try adding restaurants and restaurant reviews
1. Navigate to the Cosmos DB in the portal and review the items in the created container
