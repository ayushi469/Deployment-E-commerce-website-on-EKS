FROM mcr.microsoft.com/dotnet/sdk:10.0 AS dotnet_stage 

WORKDIR /src

COPY . /src

RUN dotnet restore 
RUN dotnet publish -c Release -o /dotnet/publish

FROM mcr.microsoft.com/dotnet/aspnet:10.0

WORKDIR /app

COPY --from=dotnet_stage /dotnet/publish .

CMD ["dotnet", "api.dll"]

EXPOSE 8080