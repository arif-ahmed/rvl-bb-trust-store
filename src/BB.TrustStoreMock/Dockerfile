# Multi-stage build for BB.TrustStoreMock. The build context must be the
# project folder (Render sets rootDir: src/BB.TrustStoreMock) so COPY . .
# brings in the csproj + source tree.
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore BB.TrustStoreMock.csproj \
 && dotnet publish BB.TrustStoreMock.csproj -c Release -o /app /p:UseAppHost=false

# Runtime: ASP.NET shared-base image. The mock exposes port 10000 to match
# Render's expected port; the platform reads ASPNETCORE_URLS to override.
FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app
COPY --from=build /app .
EXPOSE 10000
ENTRYPOINT ["dotnet", "BB.TrustStoreMock.dll"]
