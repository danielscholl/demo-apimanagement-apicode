# Instructions

__Requirements:__

1. [Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)

1. [Dotnet Core](https://www.microsoft.com/net/learn/get-started-with-dotnet-tutorial)

```powershell
dotnet --version

# Result
  2.1.301
```

1. [Docker for Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)

```powershell
docker --version

# Result
  Docker version 18.06.0-ce-rc3, build cbfa3d9
```

1. [Azure CLI](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.5.0)

```powershell
az --version

# Result
  azure-cli (2.0.42)
```

__Build:__

```powershell
dotnet restore Demo.API.sln
dotnet build Demo.API.sln

docker-compose build
docker-compose up -d

# http://localhost:8080

# Assuming you have a docker account
docker-compose push
```


### Deploy (bash cloud shell)

```bash
Resource_Group="demo"
Prefix="75098"
Registry="danielscholl"

# Create Resource Group
az group create --name ${Resource_Group} \
	--location "eastus2"

# Deploy as a Container Instance
az container create --name "${Prefix}-api" \
	--resource-group ${Resource_Group} \
	--image "${Registry}/demoapi" \
	--dns-name-label "${Prefix}-api" \
	--ports 80

az appservice plan create --name "${Prefix}-plan" \
	--resource-group ${Resource_Group} \
	--sku S1 --is-linux

az webapp create --name "${Prefix}-web" \
	--resource-group ${Resource_Group} \
	--plan "${Prefix}-plan" \
	-i "${Registry}/demoapi"

```