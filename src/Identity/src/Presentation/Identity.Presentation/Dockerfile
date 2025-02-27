#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Identity/src/Presentation/Identity.Presentation/Identity.Presentation.csproj", "src/Identity/src/Presentation/Identity.Presentation/"]
COPY ["src/Identity/src/Domain/Identity.Domain/Identity.Domain.csproj", "src/Identity/src/Domain/Identity.Domain/"]
COPY ["src/Identity/src/Application/Identity.Application/Identity.Application.csproj", "src/Identity/src/Application/Identity.Application/"]
COPY ["src/Identity/src/Infrastructure/Identity.Infrastructure/Identity.Infrastructure.csproj", "src/Identity/src/Infrastructure/Identity.Infrastructure/"]
RUN dotnet restore "./src/Identity/src/Presentation/Identity.Presentation/Identity.Presentation.csproj"
COPY . .
WORKDIR "/src/src/Identity/src/Presentation/Identity.Presentation"
RUN dotnet build "./Identity.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Identity.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Identity.Presentation.dll"]