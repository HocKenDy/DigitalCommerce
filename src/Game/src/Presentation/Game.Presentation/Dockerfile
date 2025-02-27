#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Game/src/Presentation/Game.Presentation/Game.Presentation.csproj", "src/Game/src/Presentation/Game.Presentation/"]
COPY ["src/Game/src/Domain/Game.Domain/Game.Domain.csproj", "src/Game/src/Domain/Game.Domain/"]
COPY ["src/Game/src/Application/Game.Application/Game.Application.csproj", "src/Game/src/Application/Game.Application/"]
COPY ["src/Game/src/Infrastructure/Game.Infrastructure/Game.Infrastructure.csproj", "src/Game/src/Infrastructure/Game.Infrastructure/"]
RUN dotnet restore "./src/Game/src/Presentation/Game.Presentation/Game.Presentation.csproj"
COPY . .
WORKDIR "/src/src/Game/src/Presentation/Game.Presentation"
RUN dotnet build "./Game.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Game.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Game.Presentation.dll"]