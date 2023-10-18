# DOCKER AND AZURE CONTAINER INSTANCES WORKSHOP - 2

## Create a container image for deployment to Azure Container Instances

1. Clone the git repository.

```bash
git clone https://github.com/cigdemkadakoglu/mynodejs-project.git
```

2. Define a variable for the image name.

```bash
IMAGE_NAME=mynodejsprojectgit
```

3. Create the Docker image.

```bash
docker build -t $IMAGE_NAME .
```

4. See the Docker images in the system.

```bash
docker images
```

5. Create and run the container.

```bash
docker run --name mycontainer -d -p 8080:8082 $IMAGE_NAME
```

6. See running containers in the system.

```bash
docker ps 
```

7. Define variables for reosurce group name and location.

```bash
RESOURCE_GROUP_NAME=rg-demo
LOCATION=westeurope
```

8. Create a resource group.

```bash
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION 
```

9. Define a variable for Azure Container Registry name.

```bash
ACR_NAME=acrcloudevent
```

10. Create a container registry.

```bash
az acr create --resource-group $RESOURCE_GROUP_NAME --name $ACR_NAME --sku Basic --admin-enabled true 
```

11. Define a service principal name.

```bash
SERVICE_PRINCIPAL_NAME=az-sp-cloudevent-cigdem
```

12. Define the ID of the Azure Container Registry.

```bash
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query “id” --output tsv)
```

13. Define the password of the service principal.

```bash
SERVICE_PRINCIPAL_PASSWORD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role acrpull --role acrpush --query “password” --output tsv)
```

14. Define the ID of the service principal.

```bash
SERVICE_PRINCIPAL_ID=$(az ad sp list --display-name $SERVICE_PRINCIPAL_NAME --query “[].appId” --output tsv)
```

15. See the output of the service principal ID and password.

```bash
echo "Service principal Id : $SERVICE_PRINCIPAL_ID"
echo "Service Principal Password : $SERVICE_PRINCIPAL_PASSWORD"
```

16. See the repository of the ACR.

```bash
az acr repository list --name $ACR_NAME --output table
```

17. Define a variable for Azure Container Instances name.

```bash
ACI_NAME=cigdem-aci-demo
```

18. Create a container instance.

```bash
az container create --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME --image $ACR_NAME/$IMAGE_NAME--registry-username $SERVICE_PRINCIPAL_ID --registry-password $SERVICE_PRINCIPAL_PASSWORD --ip-address Public --dns-name-label $ACI_NAME --port 8082 --cpu 2 --memory 2
```

19. Check the FQDN of the ACI.

```bash
az container show --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME --query ipAddress.fqdn
```

20. Check the log of the container.

```bash
az container logs --resource-group $RESOURCE_GROUP_NAME --name $ACI_NAME
```

21. Delete the resource group.

```bash
az group delete --name $RESOURE_GROUP_NAME
```