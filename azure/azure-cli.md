# Azure CLI

## Logging in/Out

```cmd
az login
az logout
```

## Resource Group

### Create a resource group

```
az create group --name myresourcegroupname --location "northeurope"
az create group -n myresourcegroupname -l "northeurope"
```

### Delete a resource group

```cmd
az group delete --name myresourcegroupname -y
az group delete -n myresourcegroupname -y
```



``` cmd
az deployment group create --resource-group myResourceGroup --template-file <path-to-template>
```


```
az group create --name bicep-demo --location "northeurope"
&& az deployment group create --resource-group bicep-demo --template-file test-web.bicep --parameters test-web.demo.parameters.json
```