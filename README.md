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

## We can check the WebAPI Authorization in the Weatherforecast action

If we include the **Authorize** attribute we can include the authorization required in the **WeatherForecast** action in thside the **WeatherForecastController**

It is also mandatory to include the library reference **Microsoft.AspNetCore.Authorization**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/a0c19a1e-b20b-4001-a8bb-d3724a06260e)

## To include the "Authorize" button in the WebAPI

```csharp
builder.Services.AddSwaggerGen(options =>
{
  options.AddSecurityDefinition("oauth2", new OpenApiSecurityScheme
  {
      In = Parameter.Header,
      Name = "Authorization",
      Type = SecurityScheme.ApiKey
  });

  options.OperationFilter<SecurityRequirementsOperationFilter>();
});
```

We also need to install this library **Swashbuckle.AspNetCore.Filters**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/a7ffb6e2-69aa-486e-a073-9942be3ab492)

We can also see a lock icon in the Weatherforecast action

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/1d213c3f-8697-4429-844a-ab9cc6a06aaf)

Now if we login we can obtain the access Token

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/435585f4-7a7d-417b-a2d8-cd72dd7b5f65)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/0102b098-c143-4099-9d63-b9bcb128fe3b)

If we codpy the access token and we press the Authorize button and we input **Bearer access token** now we can access the Weatherforecast WebAPI

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/7b161115-e046-4a5b-945f-e69995d9b9a1)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/468bce92-4bdb-43c0-a4e0-e739ec561321)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/f7604a31-de49-4dd3-a1f6-700afa6a1cda)


