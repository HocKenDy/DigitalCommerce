#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/Loyalty/src/Presentation/Loyalty.Presentation/Loyalty.Presentation.csproj", "src/Loyalty/src/Presentation/Loyalty.Presentation/"]
COPY ["src/Loyalty/src/Domain/Loyalty.Domain/Loyalty.Domain.csproj", "src/Loyalty/src/Domain/Loyalty.Domain/"]
COPY ["src/Loyalty/src/Application/Loyalty.Application/Loyalty.Application.csproj", "src/Loyalty/src/Application/Loyalty.Application/"]
COPY ["src/Loyalty/src/Infrastructure/Loyalty.Infrastructure/Loyalty.Infrastructure.csproj", "src/Loyalty/src/Infrastructure/Loyalty.Infrastructure/"]
RUN dotnet restore "./src/Loyalty/src/Presentation/Loyalty.Presentation/Loyalty.Presentation.csproj"
COPY . .
WORKDIR "/src/src/Loyalty/src/Presentation/Loyalty.Presentation"
RUN dotnet build "./Loyalty.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Loyalty.Presentation.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Loyalty.Presentation.dll"]