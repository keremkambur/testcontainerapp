FROM mcr.microsoft.com/dotnet/sdk:7.0-alpine AS build
WORKDIR /src
COPY ["testcontainerwasmapp/testcontainerwasmapp.csproj", "testcontainerwasmapp/"]
RUN dotnet restore "testcontainerwasmapp/testcontainerwasmapp.csproj"
COPY . .
WORKDIR "/src/testcontainerwasmapp"
RUN dotnet build "testcontainerwasmapp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "testcontainerwasmapp.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY --from=publish /app/publish/ngnix.conf /etc/nginx/nginx.conf
EXPOSE 80
