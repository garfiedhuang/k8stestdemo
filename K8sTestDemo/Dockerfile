#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["K8sTestDemo/K8sTestDemo.csproj", "K8sTestDemo/"]
RUN dotnet restore "K8sTestDemo/K8sTestDemo.csproj"
COPY . .
WORKDIR "/src/K8sTestDemo"
RUN dotnet build "K8sTestDemo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "K8sTestDemo.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "K8sTestDemo.dll"]