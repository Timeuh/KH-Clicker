# ---------- dev ----------
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS dev
WORKDIR /app
ENV ASPNETCORE_ENVIRONMENT=Development
ENV ASPNETCORE_HTTP_PORTS=3000
ENV DOTNET_USE_POLLING_FILE_WATCHER=true
EXPOSE 3000
CMD ["dotnet", "watch", "run", "--no-launch-profile"]

# ---------- build ----------
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY *.csproj ./
RUN dotnet restore
COPY . .
RUN dotnet publish -c Release -o /out --no-restore

# ---------- runner ----------
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runner
WORKDIR /app
ENV ASPNETCORE_HTTP_PORTS=3000
COPY --from=build /out .
ENTRYPOINT ["dotnet", "KhClicker.Api.dll"]