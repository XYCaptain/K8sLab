#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["K8sLab/K8sLab.csproj", "K8sLab/"]
RUN dotnet restore "K8sLab/K8sLab.csproj"
COPY . .
WORKDIR "/src/K8sLab"
RUN dotnet build "K8sLab.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "K8sLab.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "K8sLab.dll"]