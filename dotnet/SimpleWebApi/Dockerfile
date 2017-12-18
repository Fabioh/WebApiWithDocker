FROM microsoft/aspnetcore:2.0 AS base
WORKDIR /app
EXPOSE 80

# Copy csproj and restore as distinct layers
FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . .
WORKDIR /src/
RUN dotnet build -c Release -o /app SimpleWebApi.csproj
FROM build AS publish
RUN dotnet publish -c Release -o /app SimpleWebApi.csproj

# Build runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
EXPOSE 80
ENTRYPOINT ["dotnet", "SimpleWebApi.dll"]