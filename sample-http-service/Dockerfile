#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 49153

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["sample-http-service/sample-http-service.csproj", "sample-http-service/"]
RUN dotnet restore "sample-http-service/sample-http-service.csproj"
COPY . .
WORKDIR "/src/sample-http-service"
RUN dotnet build "sample-http-service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "sample-http-service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "sample-http-service.dll"]