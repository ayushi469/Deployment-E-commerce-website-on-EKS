### This project is simple :

## we will simply deploy our 3 tire architecture application on kubernetes of AWS which is EKS and we will run CI/CD pipeline and we 
## can use database also

                    Internet
                       |
                       ↓
                  AWS ALB
                       |
                 ┌─────┴─────┐
                 ↓           ↓
              Frontend     Backend
               Tier         Tier
                 |           |
                 |           ├──────→ Redis
                 |           |
                 |           └──────→ Database
                 |
                 ↓
               Users


## This is kind of architeture we are gonna use.


## first we will build application locally.

### First install dotnet on laptop
brew install --cask dotnet-sdk.  ## to run the backend application in dotnet and c#
dotnet --version

dotnet restore
dotnet build
dotnet run

brew install node
node --version
npm --version

open -a Docker : when there is error coming while doing docker ps or any other command
Docker daemon at unix:///Users/dibs/.docker/run/docker.sock. Is the docker daemon running?

docker login

postgres docker container run command :
docker run --name postgress_database \
-e POSTGRES_USER=postgress \
-e POSTGRES_PASSWORD=mysecretpassword \
-e POSTGRES_DB=pillar \
-p 5432:5432 -d postgress

docker exec -it postgress_database psql -U postgres -d pillar

docker exec -i postgres_database psql -U postgres -d pillar < pillar.sql : to insert the data 

need to learn about CORS: Our froneted is hitting on https://localhost:5007 , so we have to add into our server to allow the frontend "https://localhost:4200"



next steps:

Dockerize everything :
1. indiviaully run all docker container, 1 container for angular , 1 is for .NET and 1 is for postgress
2. try to connect them each and try if the application is working like it is working excatly like locally
