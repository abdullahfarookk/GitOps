# FROM mcr.microsoft.com/dotnet/aspnet:7.0-alpine-arm64v8 AS base
# WORKDIR /app
# EXPOSE 80
# EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Frontend.csproj", "./"]
RUN dotnet restore "Frontend.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "Frontend.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Frontend.csproj" -c Release -o /app/publish

FROM arm64v8/nginx:stable-alpine AS final
EXPOSE 80
EXPOSE 443
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf

# FROM base AS final
# WORKDIR /app
# COPY --from=publish /app/publish .
# ENTRYPOINT ["dotnet", "Frontend.dll"]
