# dockard
Setting up offline registry with minimal set of images

# Setup test

## Local registry
 docker run -d -p 5000:5000 --restart always --name registry registry:2
check that its up by hitting
```http://localhost:5000/v2/_catalog```

should get

```{"repositories":[]}```

dki pull  mcr.microsoft.com/dotnet/aspnet:3.1-alpine
dki pull mcr.microsoft.com/dotnet/sdk:3.1

dki tag mcr.microsoft.com/dotnet/sdk:3.1 localhost:5000/mcr.microsoft.com/dotnet/sdk:3.1
dki tag mcr.microsoft.com/dotnet/aspnet:3.1-alpine localhost:5000/mcr.microsoft.com/dotnet/aspnet:3.1
dki push localhost:5000/mcr.microsoft.com/dotnet/sdk:3.1
dki push localhost:5000/mcr.microsoft.com/dotnet/aspnet:3.1

Check the catalog, should now have

{"repositories":["mcr.microsoft.com/dotnet/aspnet","mcr.microsoft.com/dotnet/sdk"]}

Clear out images from repo 
dki rm <images>
dki prune
dk builder prune

## App
Used VS to create a basic asp core app target is 3.1
auto create docker file for linux
update to use images from local registry

# Test
Disconnect network
build image
docker image build -f Contained/Dockerfile . -t test:1.0
create container
dkc create -p 8080:80 --name testapp test:1.0
dkc start testapp
Go to localhost:8080 and see if app is running

# Images
mcr.microsoft.com/dotnet/aspnet:3.1-alpine
mcr.microsoft.com/dotnet/sdk:3.1





