FROM localhost:5000/mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM localhost:5000/mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["Contained/Contained.csproj", "Contained/"]
RUN dotnet restore "Contained/Contained.csproj"
COPY . .
WORKDIR "/src/Contained"
RUN dotnet build "Contained.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Contained.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Contained.dll"]
