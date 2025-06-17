# See https://aka.ms/customizecontainer to learn how to customize your debug container 
# and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Stage 1: Base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Stage 2: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy .csproj file and restore dependencies
COPY ["PidrobitOK.Moderator/PidrobitOK.ModeratorService/PidrobitOK.ModeratorService.csproj", "PidrobitOK.Moderator/PidrobitOK.ModeratorService/"]
RUN dotnet restore "PidrobitOK.Moderator/PidrobitOK.ModeratorService/PidrobitOK.ModeratorService.csproj"

# Copy the entire source
COPY . .

# Build the application
WORKDIR "/src/PidrobitOK.Moderator/PidrobitOK.ModeratorService"
RUN dotnet build "PidrobitOK.ModeratorService.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Stage 3: Publish
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "PidrobitOK.ModeratorService.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage 4: Final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PidrobitOK.ModeratorService.dll"]
