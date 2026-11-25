# AZD Template for Node.js with a secure App Service and Azure DocumentDB architecture

This is an infrastructure-only template that doesn't include any application code. You can use it to quickly create the resources you need to deploy a data-driven Node.js web app, such as Express.js, to Azure. For an example, see [Tutorial: Deploy a Node.js + MongoDB web app to Azure](https://learn.microsoft.com/azure/app-service/tutorial-nodejs-mongodb-app). It provisions the following resources in an Azure resource group:

- A [virtual network](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview).
- A VNET-integrated [App Service](https://learn.microsoft.com/azure/app-service/overview) app.
- An [Azure DocumentDB](https://learn.microsoft.com/azure/documentdb/overview) managed cluster, accessible only from within the virtual network.
- An [Azure Managed Redis](https://learn.microsoft.com/azure/redis/overview) instance, accessible only from within the virtual network.
- An [Azure Key Vault](https://learn.microsoft.com/azure/key-vault/general/overview), which manages secrets for App Service.
- A [Log Analytics workspace](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-workspace-overview) to ship App Service logs.

**New environments only:** this changes the database resource to
`Microsoft.DocumentDB/mongoClusters`; it does not migrate data from Cosmos DB RU-based
accounts. Do not reuse an existing RU environment/resource group. Review the
[infrastructure, credentials, pricing, rotation, validation, and cleanup guide](infra/README.md).
The published template identifier below is retained for compatibility; it is not the
database service name.

Your app must read `AZURE_DOCUMENTDB_CONNECTIONSTRING` and select the database specified
by `AZURE_DOCUMENTDB_DATABASE` (default `todolist`). For Mongoose, pass the latter as
`{ dbName: process.env.AZURE_DOCUMENTDB_DATABASE }` to `mongoose.connect`. This
infrastructure-only repository cannot update your application's connection code.

## Quick deployment

To deploy your data-driven Node.js web app, make sure you have [installed AZD](https://learn.microsoft.com/azure/developer/azure-developer-cli/install-azd), and run the following commands and sequence and follow the prompts:

```shell
# CD to your app's repo root
cd <repo-root>
# Initialize a new azd environment with this template
azd init --template nodejs-app-service-cosmos-redis-infra
# Choose a NEW environment name; supply the password at the secure prompt.
# Provision resources and deploy application code
azd up
```

When `azd` has finished deploying, you'll see some helpful output as provided by the template

- An endpoint URI for the web app.
- A link to the resource group in the Azure portal.
- The explicit DocumentDB app setting names, and link to the app settings page in the Azure portal.
- A link to open an SSH session with the web app. This is useful to run database migrations manually.
- A link to the App Service logs.

When you've made any changes to your application code, you can just run:

```shell
azd deploy
```

## Getting help

If you're working with this project and running into issues, please post in [Issues](/issues).