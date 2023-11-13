# Play.Catalog
Play Economy Catalog microservice

## Create and publish package
```powershell
$version="1.0.2"
$owner="WallaWast"
$gh_pat="[PAT HERE]"

dotnet pack src\Play.Catalog.Contracts\ --configuration Release -p:PackageVersion=$version -p:RepositoryUrl=https://github.com/$owner/play.catalog -o ..\packages

dotnet nuget push ..\packages\Play.Catalog.Contracts.$version.nupkg --api-key $gh_pat --source "github"
```

## Build the docker image
```powershell
$env:GH_OWNER="WallaWast"
$env:GH_PAT="[PAT HERE]"
docker build --secret id=GH_OWNER --secret id=GH_PAT -t play.catalog:$version .
```

## Run the docker image
```powershell
$adminPass="[PASSWORD HERE]"
docker run -it --rm -p 5000:5000 --name catalog -e MongoDbSettings__Host=mongo -e ServiceSettings__Authority=http://localhost:5002 -e RabbitMQSettings__Host=rabbitmq -e IdentitySettings__AdminUserPassword=$adminPass --network playinfra_default play.catalog:$version
```