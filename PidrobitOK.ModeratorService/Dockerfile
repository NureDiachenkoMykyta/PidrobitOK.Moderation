# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Use ASP.NET Core runtime for the final image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Use .NET SDK for build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy .csproj and restore
COPY "PidrobitOK.ModeratorService/PidrobitOK.ModeratorService.csproj" "PidrobitOK.ModeratorService/"
RUN dotnet restore "PidrobitOK.ModeratorService/PidrobitOK.ModeratorService.csproj"

# Copy everything and build
COPY . .
WORKDIR "/src/PidrobitOK.ModeratorService"
RUN dotnet build "PidrobitOK.ModeratorService.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the app
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "PidrobitOK.ModeratorService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PidrobitOK.ModeratorService.dll"]
