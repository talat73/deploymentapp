FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["DeploymentCICDWithGithub/DeploymentCICDWithGithub.csproj", "DeploymentCICDWithGithub/"]
RUN dotnet restore "DeploymentCICDWithGithub/DeploymentCICDWithGithub.csproj"
COPY . .
WORKDIR "/src/DeploymentCICDWithGithub"
RUN dotnet build "DeploymentCICDWithGithub.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "DeploymentCICDWithGithub.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DeploymentCICDWithGithub.dll"]
