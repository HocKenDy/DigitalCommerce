#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/CRM/src/Presentation/CRM.Presentation/CRM.Presentation.csproj", "src/CRM/src/Presentation/CRM.Presentation/"]
COPY ["src/CRM/src/Domain/CRM.Domain/CRM.Domain.csproj", "src/CRM/src/Domain/CRM.Domain/"]
COPY ["src/CRM/src/Application/CRM.Application/CRM.Application.csproj", "src/CRM/src/Application/CRM.Application/"]
COPY ["src/CRM/src/Infrastructure/CRM.Infrastructure/CRM.Infrastructure.csproj", "src/CRM/src/Infrastructure/CRM.Infrastructure/"]
RUN dotnet restore "./src/CRM/src/Presentation/CRM.Presentation/CRM.Presentation.csproj"
COPY . .
WORKDIR "/src/src/CRM/src/Presentation/CRM.Presentation"
RUN dotnet build "./CRM.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./CRM.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CRM.Presentation.dll"]