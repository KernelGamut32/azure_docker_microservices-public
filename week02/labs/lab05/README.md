# Lab 05 - [Multi-Container Group Using a YAML File](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-multi-container-yaml)

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
1. Follow along with the steps outlined in the tutorial
1. Use `az group list --query [*].name` (https://learn.microsoft.com/en-us/cli/azure/query-azure-cli) to get the name of the available resource group
1. Do not create a new resource group - use the one provided in the sandbox (from the previous instruction)
