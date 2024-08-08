FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG NUGET_USER
ARG NUGET_PASS

WORKDIR /src
COPY . .

RUN dotnet nuget add source https://nuget.pkg.github.com/nirsoft-dev/index.json -n github --store-password-in-clear-text -u $NUGET_USER -p $NUGET_PASS
RUN dotnet restore "BlazorApp/BlazorApp.csproj"

FROM build AS publish
RUN dotnet publish "BlazorApp/BlazorApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
EXPOSE 80
ENTRYPOINT ["dotnet", "BlazorApp.dll"]