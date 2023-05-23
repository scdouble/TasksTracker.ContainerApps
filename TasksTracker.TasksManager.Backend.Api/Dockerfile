FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 5223

ENV ASPNETCORE_URLS=http://+:5223

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["TasksTracker.TasksManager.Backend.Api/TasksTracker.TasksManager.Backend.Api.csproj", "TasksTracker.TasksManager.Backend.Api/"]
RUN dotnet restore "TasksTracker.TasksManager.Backend.Api/TasksTracker.TasksManager.Backend.Api.csproj"
COPY . .
WORKDIR "/src/TasksTracker.TasksManager.Backend.Api"
RUN dotnet build "TasksTracker.TasksManager.Backend.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TasksTracker.TasksManager.Backend.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TasksTracker.TasksManager.Backend.Api.dll"]
