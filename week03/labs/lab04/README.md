# Lab 04 - Multiple Microservices in Azure

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
export RG=$(az group list --query [*].name --output tsv)
export LOC=$(az group list --query [*].location --output tsv)
export DB=toyshop$RANDOM
export REG=toyshop$RANDOM
```

6. Do not create a new resource group - use the one provided in the sandbox
1. Create a new ACR using `az acr create --resource-group $RG --location $LOC --sku Basic --name $REG --admin-enabled true`
1. Execute `az acr list --query [*].name` (or review in Azure Portal) to confirm creation
1. You will not be able to create a new Service Principal for your ACR (due to restrictions in ACG), so use the following as the steps instead:
    - Execute `export USR=$(az acr credential show --name $REG --query username --output tsv)` to gather username for accessing ACR into a new environment variable
    - Execute `export PASSWD=$(az acr credential show --name $REG --query passwords[0].value --output tsv)` to gather password for accessing ACR into a new environment variable
1. From `Cloud Shell`, execute `git clone https://github.com/KernelGamut32/azure_docker_microservices-public.git` to pull down the class repository
1. Navigate to the `users` microservice folder using `cd azure_docker_microservices-public/week03/labs/lab04/users`
1. Create a `Dockerfile` that can be used to generate and store an image in ACR for the `users` microservices
1. Run `az acr build --registry $REG --resource-group $RG --image users-svc:latest .` to build an image for the microservice
1. Repeat for both the `inventory` and `toyshop` microservices using `inventory-svc:latest` and `toyshop-svc:latest` for image identifiers
1. Navigate to the `Repositories` area of your ACR to confirm creation of the images
1. Use the following to create a new containerized instance of a MySQL database in ACI:

```
az container create \
    --name mysql \
    --resource-group $RG \
    --image mysql \
    --environment-variables MYSQL_ROOT_PASSWORD=pass123  \
    --ip-address public \
    --port=3306
```

17. Use the following to create a new containerized instance of the `users` microservice in ACI:

```
az container create \
    --name users-svc \
    --resource-group $RG \
    --registry-login-server $REG.azurecr.io \
    --registry-username $USR \
    --registry-password $PASSWD \
    --image $REG.azurecr.io/users-svc:latest \
    --environment-variables MYSQL_USERNAME=root MYSQL_SERVICE_HOST=$(az container show --name mysql --resource-group $RG --query ipAddress.ip --output tsv) MYSQL_PASSWORD=pass123 MYSQL_DATABASE=users_db \
    --ip-address public \
    --port=5050 \
    --dns-name-label userssvc$RANDOM
```

18. Use `az container show --name users-svc --resource-group $RG --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table` to get FQDN for the `users` microservice container
1. Use the following to create a new containerized instance of the `inventory` microservice in ACI:

```
az container create \
    --name inventory-svc \
    --resource-group $RG \
    --registry-login-server $REG.azurecr.io \
    --registry-username $USR \
    --registry-password $PASSWD \
    --image $REG.azurecr.io/inventory-svc:latest \
    --environment-variables MYSQL_USERNAME=root MYSQL_SERVICE_HOST=$(az container show --name mysql --resource-group $RG --query ipAddress.ip --output tsv) MYSQL_PASSWORD=pass123 MYSQL_DATABASE=inventory_db \
    --ip-address public \
    --port=5100 \
    --dns-name-label inventorysvc$RANDOM
```

20. Use `az container show --name inventory-svc --resource-group $RG --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table` to get FQDN for the `inventory` microservice container
1. Use the following to create a new containerized instance of the `toyshop` microservice in ACI:

```
az container create \
    --name toyshop-svc \
    --resource-group $RG \
    --registry-login-server $REG.azurecr.io \
    --registry-username $USR \
    --registry-password $PASSWD \
    --image $REG.azurecr.io/toyshop-svc:latest \
    --environment-variables MYSQL_USERNAME=root MYSQL_SERVICE_HOST=$(az container show --name mysql --resource-group $RG --query ipAddress.ip --output tsv) MYSQL_PASSWORD=pass123 MYSQL_DATABASE=toyshop_db AUTH_ENDPOINT="http://$(az container show --name users-svc --resource-group $RG --query ipAddress.fqdn --out tsv):5050/auth/verify" INV_ENDPOINT="http://$(az container show --name inventory-svc --resource-group $RG --query ipAddress.fqdn --out tsv):5100/inventory" \
    --ip-address public \
    --port=5000 \
    --dns-name-label toyshopsvc$RANDOM
```

22. Use `az container show --name toyshop-svc --resource-group $RG --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table` to get FQDN for the `toyshop` microservice container
1. Using POSTMan, cURL, or another tool of your choosing, test each of the microservice endpoints; there is a POSTMan collection provided in the `supporting-docs` folder
1. For testing, you'll use http and the FQDN for each service gathered from the previous steps; see [How to add SSL to Azure Container Instance App?](https://stackoverflow.com/questions/60958057/how-to-add-ssl-to-azure-container-instance-app) for information on adding TLS to an ACI container