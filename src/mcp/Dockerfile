# Build stage
#Load the dotnet sdk which is needed later on for Run. Publish, etc.  Create /src temp workspace and change into that directory
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Copy ServiceDefaults project first
COPY ["ContosoTravel.ServiceDefaults/ContosoTravel.ServiceDefaults.csproj", "ContosoTravel.ServiceDefaults/"]

# Copy mcp project
COPY ["mcp/ContosoTravel.McpServer.csproj", "mcp/"]

# Restore dependencies to download all of the packages
RUN dotnet restore "mcp/ContosoTravel.McpServer.csproj"

# Copy everything else.  Copy all of the code
COPY ["ContosoTravel.ServiceDefaults/", "ContosoTravel.ServiceDefaults/"]
COPY ["mcp/", "mcp/"]

# Build and publish
WORKDIR "/src/mcp"
RUN dotnet publish "ContosoTravel.McpServer.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
EXPOSE 8080

# Copy published app
COPY --from=build /app/publish .

# Set environment variables
ENV ASPNETCORE_URLS=http://+:8080
ENV PORT=8080

#Launch these when the docker container is started
ENTRYPOINT ["dotnet", "ContosoTravel.McpServer.dll"]
