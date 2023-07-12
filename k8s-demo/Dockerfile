#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["k8s-demo/k8s-demo.csproj", "k8s-demo/"]
RUN dotnet restore "k8s-demo/k8s-demo.csproj"
COPY . .
WORKDIR "/src/k8s-demo"
RUN dotnet build "k8s-demo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "k8s-demo.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "k8s-demo.dll"]