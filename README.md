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


## Register a new User

We send the following request for registry a new user:

```json
{
  "email": "luiscocoenriquez@hotmail.com",
  "password": "Test123@"
}
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/6784701c-32f4-4578-8721-aa0a03bdea3b)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/5a3ac255-7100-4d45-9e55-2004fb4f9b1a)

## Login

We login with the following credentials:

```json
{
  "email": "luiscocoenriquez@hotmail.com",
  "password": "Test123@",
  "twoFactorCode": "string",
  "twoFactorRecoveryCode": "string"
}
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/2f10d949-7ab7-437c-89ca-4c3ebd379592)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/7e028ce6-c3e3-4352-9302-4b9d1e4569b5)




