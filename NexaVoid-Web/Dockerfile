FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["NexaVoid-Web/NexaVoid-Web.csproj", "NexaVoid-Web/"]
RUN dotnet restore "NexaVoid-Web/NexaVoid-Web.csproj"
COPY . .
WORKDIR "/src/NexaVoid-Web"
RUN dotnet build "NexaVoid-Web.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "NexaVoid-Web.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NexaVoid-Web.dll"]
