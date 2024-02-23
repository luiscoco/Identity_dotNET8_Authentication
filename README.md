# dotNET8Authentication

## 1. Prerequisites

### 1.1. How to pull and run the SQL Server Docker container 

```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Luiscoco123456" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

### 1.2. Create Azure SQL database for identity


## 2. Create WebAPI in Visual Studio 2022 Community Edition




## 3. Load project dependencies

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/9af00e2c-79ac-4c99-a989-784d65b031c1)

## 4. Create the DataContext.cs

We have to create the **Data** foder and inside to create a new file DataContext.cs

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace dotNET8Authentication.Data
{
    public class DataContext : IdentityDbContext
    {
        public DataContext(DbContextOptions<DataContext> options) : base(options)
        { 
        
        }
    }
}
```

## 5. Modify the program.cs

```csharp
using dotNET8Authentication.Data;
using Microsoft.AspNetCore.Identity;
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using Swashbuckle.AspNetCore.Filters;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(options =>
{
    options.AddSecurityDefinition("oauth2", new OpenApiSecurityScheme
    {
        In = ParameterLocation.Header,
        Name = "Authorization",
        Type = SecuritySchemeType.ApiKey
    });

    options.OperationFilter<SecurityRequirementsOperationFilter>();
});

builder.Services.AddDbContext<DataContext>(options =>
{
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"));
});

builder.Services.AddAuthorization();

builder.Services.AddIdentityApiEndpoints<IdentityUser>()
    .AddEntityFrameworkStores<DataContext>();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.MapIdentityApi<IdentityUser>();

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

## 6. Modify the appsettings.json 

```json
{
  "Logging": {
    "LogLevel": {
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost,1433;Database=DotNet8Authentication;User ID=sa;Password=Luiscoco123456;Encrypt=false;TrustServerCertificate=true;"
  },
  "AllowedHosts": "*"
}
```

## 7. Create the database migration and update the database

These are the EntityFramework commands we have to run

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


## 8. How to test the application

Build and run the application with Visual Studio 2022

### 8.1. Register a new User

We send the following request for registry a new user:

```json
{
  "email": "luiscocoenriquez@hotmail.com",
  "password": "Test123@"
}
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/6784701c-32f4-4578-8721-aa0a03bdea3b)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/5a3ac255-7100-4d45-9e55-2004fb4f9b1a)

### 8.2. Login

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

### 8.3. Check the WebAPI Authorization in the Weatherforecast

If we include the **Authorize** attribute we can include the authorization required in the **WeatherForecast** action in thside the **WeatherForecastController**

It is also mandatory to include the library reference **Microsoft.AspNetCore.Authorization**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/a0c19a1e-b20b-4001-a8bb-d3724a06260e)

### 8.4. To include the "Authorize" button in the WebAPI

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

## 9. Authorization with Cookies


## 10. References

.NET 8 Authentication with Identity in a Web API with Bearer Tokens & Cookies (youtube video): 

https://www.youtube.com/watch?v=8J3nuUegtL4

.NET 8 🔥🚀 : Guide to Secure User Authentication - Exploring Identity new Features:

https://www.youtube.com/watch?v=ORzt0lks2H4

Coding Short: Using Bearer Tokens in .NET 8 Identity (youtube video):

https://www.youtube.com/watch?v=owoy6DG0UG0

.NET 7 Web API 🔒 Create JSON Web Tokens (JWT) - User Registration / Login / Authentication (youtube video):

https://www.youtube.com/watch?v=UwruwHl3BlU

.NET 6 Web API 🔒 Create JSON Web Tokens (JWT) - User Registration / Login / Authentication:

https://www.youtube.com/watch?v=v7q3pEK1EA0



