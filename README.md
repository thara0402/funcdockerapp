# Azure Function on Linux

## azure-functions-core-tools

```shell-session
$ sudo npm install -g azure-functions-core-tools@core --unsafe-perm true
```
https://github.com/Azure/azure-functions-cli

## Create Function

```shell-session
$ func init funcdockerapp --docker --sample
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in /Users/thara/Develop/funcdockerapp/.git/

Writing Dockerfile
Writing HttpFunction/function.json
Writing HttpFunction/index.js

Next Steps:
Run> func start
To start your functions, then hit the URL displayed for the function.

Run> docker build -t <image name> .
to build a docker image with your functions

Run> docker run -p 8080:80 -it <image name>
To run the container then trigger your function on port 8080.
```

```shell-session
$ docker build -t thara0402/funcapp:v0.1 .
$ docker run -p 8080:80 -it thara0402/funcapp:v0.1
```

http://localhost:8080/api/HttpFunction?name=gooner

## Deploy

```shell-session
$ docker push thara0402/funcapp:v0.1

$ az group create --name gooner0111 --location westeurope
$ az storage account create --name gooner0111 --location westeurope --resource-group gooner0111 --sku Standard_LRS
$ az appservice plan create --name gooner0111 --resource-group gooner0111 --sku S1 --is-linux

$ az functionapp create --name gooner0111 --storage-account gooner0111 --resource-group gooner0111 \
--plan gooner0111 --deployment-container-image-name thara0402/funcapp:v0.1

$ storageConnectionString=$(az storage account show-connection-string \
--resource-group gooner0111 --name gooner0111 \
--query connectionString --output tsv)

$ az functionapp config appsettings set --name gooner0111 \
--resource-group gooner0111 \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString

```


