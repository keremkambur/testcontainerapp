FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["testcontainerapp/testcontainerapp.csproj", "testcontainerapp/"]
RUN dotnet restore "testcontainerapp/testcontainerapp.csproj"
COPY . .
WORKDIR "/src/testcontainerapp"
RUN dotnet build "testcontainerapp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "testcontainerapp.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "testcontainerapp.dll"]
