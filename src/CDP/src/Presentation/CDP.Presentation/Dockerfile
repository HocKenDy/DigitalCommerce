#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/CDP/src/Presentation/CDP.Presentation/CDP.Presentation.csproj", "src/CDP/src/Presentation/CDP.Presentation/"]
COPY ["src/CDP/src/Domain/CDP.Domain/CDP.Domain.csproj", "src/CDP/src/Domain/CDP.Domain/"]
COPY ["src/CDP/src/Application/CDP.Application/CDP.Application.csproj", "src/CDP/src/Application/CDP.Application/"]
COPY ["src/CDP/src/Infrastructure/CDP.Infrastructure/CDP.Infrastructure.csproj", "src/CDP/src/Infrastructure/CDP.Infrastructure/"]
RUN dotnet restore "./src/CDP/src/Presentation/CDP.Presentation/CDP.Presentation.csproj"
COPY . .
WORKDIR "/src/src/CDP/src/Presentation/CDP.Presentation"
RUN dotnet build "./CDP.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./CDP.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CDP.Presentation.dll"]