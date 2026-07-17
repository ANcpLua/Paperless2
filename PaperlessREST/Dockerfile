FROM mcr.microsoft.com/dotnet/aspnet:8.0@sha256:9822a6201c64c9f38b8a190832a7ae21cd30150d509b3ea170fd83b4f4e6166a AS base
WORKDIR /app
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0@sha256:89ce6291bde9acdf59594e79fb8277c6d84c46e4b1f5bf126a4f18766e4bd597 AS build
WORKDIR /src

COPY ["PaperlessREST/PaperlessREST.csproj", "PaperlessREST/"]
COPY ["Contract/Contract.csproj", "Contract/"]
RUN dotnet restore "PaperlessREST/PaperlessREST.csproj"

COPY . .
WORKDIR "/src/PaperlessREST"
RUN dotnet build "PaperlessREST.csproj" -c Release -o /app/build


FROM build AS publish
RUN dotnet publish "PaperlessREST.csproj" \
    -c Release \
    -r linux-x64 \
    --no-self-contained \
    -o /app/publish


FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
COPY PaperlessREST/rest-appsettings.json /app/rest-appsettings.json

ENTRYPOINT ["dotnet", "PaperlessREST.dll"]
