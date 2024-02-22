# dotNET8Authentication

## How to pull and run the SQL Server Docker container 

```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Luiscoco123456" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

## EntityFramework commands

```
dotnet tool install --global dotnet-ef
```

```
dotnet ef
```

```
dotnet ef migrations add initial
```

```
dotnet ef database update
```

## IMPORTANT NOTE

In the project file set the variable "InvariantGlobalization" to "false"
