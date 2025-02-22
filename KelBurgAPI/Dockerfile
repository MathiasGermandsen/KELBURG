FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

COPY ["KelBurgAPI/KelBurgAPI.csproj", "KelBurgAPI/"]
RUN dotnet restore "KelBurgAPI/KelBurgAPI.csproj"

COPY . .
WORKDIR "/src/KelBurgAPI"
RUN dotnet build "KelBurgAPI.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "KelBurgAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "KelBurgAPI.dll"]
