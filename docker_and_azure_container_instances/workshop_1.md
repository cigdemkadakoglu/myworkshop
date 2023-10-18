# DOCKER AND AZURE CONTAINER INSTANCES WORKSHOP - 1

## Deploy a container instance in Azure using the Azure CLI

1 . Define variables for reosurce group name and location.

```bash
RESOURCE_GROUP_NAME=rg-demo
LOCATION=westeurope
```

2. Create a resource group.

```bash
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION 
```

3. Define a variable for Azure Container Instances name.

```bash
ACI_NAME=cigdem-aci-demo
```

4. Create a container instance.

```bash
az container create --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME --image mcr.microsoft.com/azuredocs/aci-helloworld --dns-name-label $ACI_NAME --ports 80
```

5. Check the status of the container.

```bash
az container show --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME --query “{FQDN : ipAddress.fqdn, ProvisioningState : provisioningState }” --out table 
```

6. Check the log of the container.

```bash
az container logs --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME
```

7. Delete the resource group.

```bash
az group delete --name $RESOURCE_GROUP_NAME
```
