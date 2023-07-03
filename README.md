To deploy an ASP.NET Core application using a Docker container, you can follow these steps:

Prepare your ASP.NET Core application:

Ensure you have a working ASP.NET Core application.
Make sure your application runs correctly locally.
Create a Dockerfile:

Create a new file named Dockerfile in the root directory of your ASP.NET Core application.

Open the Dockerfile in a text editor and add the following content:
------------------------------------------------------------------------------------------------------
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["WebApp.csproj", "."]
RUN dotnet restore "./WebApp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "WebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApp.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp.dll"]

---------------------------------------------------------------------------------------------------------------------------------------

Test your application:

Open a web browser and navigate to http://localhost to access your ASP.NET Core application running inside the Docker container.
That's it! Your ASP.NET Core application should now be running inside a Docker container. You can deploy this container to various platforms, such as a local Docker host, cloud-based container services like Azure Container Instances or AWS ECS, or orchestration platforms like Kubernetes.
