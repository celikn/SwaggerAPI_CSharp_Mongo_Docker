FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:3.1
WORKDIR /app
COPY --from=build /app/out .
ENV ASPNETCORE_ENVIRONMENT=”development”
#https://stackoverflow.com/questions/54901609/how-to-set-the-environment-of-dotnet-core-in-docker?rq=1
#https://stackoverflow.com/questions/52376855/aspnetcore-environment-in-docker/52383707s
ENTRYPOINT ["dotnet", "SupplementCRUDAPI.dll"]