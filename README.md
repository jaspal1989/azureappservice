### Deploy a Linux hosted Azure App service using GitHub Actions.

This workflow utilizes below mentioned GitHub Actions to deploy an App Service to Azure using ARM template.

* [Checkout Action](https://github.com/marketplace/actions/checkout)
* [Azure Login Action](https://github.com/marketplace/actions/azure-login)
* [Deploy Azure Resource Manager Template Action](https://github.com/marketplace/actions/deploy-azure-resource-manager-arm-template)

The workflow `Web App Creation` will get trigerred whenever there is a change in parameter file `app-service.parameters.json` 

#### Prerequisites - 
* Generate deployment credentials
* Configure the GitHub secrets

##### Generate deployment credentials

Deployment credentials can be created using Azure CLI. Run below command using Azure Cloud Shell in the Azure portal

`az ad sp create-for-rbac --name {myApp} --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{MyResourceGroup} --sdk-auth`

> Replace the below mentioned placeholders with relevant fields.
>* {myApp} is the name of your application.
>* {subscription-id} is the id of you subscription.
>* {MyResourceGroup} is the RG where resources will be deployed.

##### Configure the GitHub secrets

Create the secrets for Azure credentials , resource group and subscription as below

* `AZURE_CREDENTIALS` - Paste the entire JSON output from the Azure CLI command into the secret's value field.
* `AZURE_RG` - Add the name of the resource group where you want to deploy the secret's value field.
* `AZURE_SUBSCRIPTION` - Add your subscription ID to the secret's value field.

#### Updating the Parameter file (app-service.parameters.json) -

The workflow will only be trigerred when the changes are merged into `main` branch.

* `webappname` is the name of your App Service.
* `appServicePlanname` is the name of app service plan if already exists or desired name if creating a new one
* `linuxFxVersion` value which can only be one of the below ones
    * PYTHON|3.8
    * DOTNETCORE|3.0
    * NODE|10.15
    * JAVA|1.8 |TOMCAT|9.0
* `skucode` value which can only be one of the below ones
    * P1v2
    * P1v3
    * P2v2
* `skutier` value has only been limited to below at this stage
    * PremiumV2


