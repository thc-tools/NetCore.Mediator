FROM mcr.microsoft.com/dotnet/aspnet:5.0-focal AS base
WORKDIR /app
EXPOSE 5444

ENV ASPNETCORE_URLS=http://+:5444

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:5.0-focal AS build
WORKDIR /src
COPY ["src/NetCore.Mediator.csproj", "src/"]
RUN dotnet restore "src/NetCore.Mediator.csproj"
COPY . .
WORKDIR "/src/src"
RUN dotnet build "NetCore.Mediator.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NetCore.Mediator.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NetCore.Mediator.dll"]
