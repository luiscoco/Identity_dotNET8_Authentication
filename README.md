# How to include Authentication (JWT & Cookies) in your .NET 8 Web API

## 1. Prerequisites

### 1.1. How to pull and run the SQL Server Docker container 

We run Docker Desktop application

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/be7cb784-9f18-4f5f-a557-cfcc47cc3d6f)

We open a command prompt window and run the following command

```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Luiscoco123456" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2022-latest
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/5661f705-44eb-4537-855e-ed04279d002e)

We can also see the image in Docker Desktop

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/399896b3-0c2b-4ec0-acfc-b83f10691e5d)

And also we can see the running container in Docker Desktop

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/9a1b0c37-5bc0-4129-987c-60f77db91448)

We run SSMS SQL Server Management Studio and we connect to the SQL Server running docker container

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/161280ed-c082-458b-b5d0-8474c050be70)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/f3b9ffd8-e3ea-4418-9e32-c63a90fa8df6)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/844b1d77-0a27-49f8-9e47-dfc3413f3af8)

In the password we input **Luiscoco123456**, the same password as we provide when we run the SQL Server docker container

Now we can create a new database "**DotNet8Authentication**" for this sample

```
CREATE DATABASE DotNet8Authentication
GO
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/e8ea8450-da3b-47da-bc05-26cf8df3cf2f)

We select the **Refresh** option in the database

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/169c0f98-cb8b-4479-9731-efc3002761a7)

Now we can see the new database in the Databases tree

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/5b1d90b3-5fec-4ef6-a528-70eabb798f98)

### 1.2. Create Azure SQL database for identity (OPTIONAL)

Another option is to create your identity database in Azure SQL database

We first have to log in to Azure Portal

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/a7b80f6e-c4b5-4768-8497-e21e9f22ddcf)

We navigate into Create a new Resource and we select **Azure SQL**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/935ed12c-7321-4bed-adbb-5e777f173367)

We select the option **SQL databases** create

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/0b7f0cba-42c0-478e-8024-c24a352a4718)

We select the button **Apply offer (preview)**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/8ab2cef5-c7c0-4826-b890-3f016ec4c80d)

We input the requested data to create the database: : Subscription, ResourceGroup, Databasename

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/676b0469-fee2-4b6d-a561-1ce45a84a38a)

We also have to create a new Server. We input the Server name and location

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/bb7552ff-b173-452d-994a-2a6739c33630)

We also select the **Use SQL authentication** option, and input the login name and password

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/0de6d222-fdef-4d60-9367-6e6efdcb3acc)

We also have to configure the database computing resources

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/d9c97754-fa36-4228-8999-e8b58781c817)

We input the required vCores and storing menory size

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/29490aa4-541a-4550-8daa-21c52b899605)

We also input the replication option and the behaviour when free offer finish

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/13c8417d-6f1f-4352-9fcc-45ff147f967a)

We also have to select the Networking options

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/89b3ada5-4106-4d15-bf6e-d36851ae1fd2)

We finally create the database

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/f0ad0927-fec4-4161-9ede-ad6d5021c898)

We go to the resource

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/3c5a0dfb-d8c0-4dac-a349-79a8922732ce)

And then we copy the database connection string and input this value in the **appsettings.json** file

```
Server=tcp:myidentityserver.database.windows.net,1433;Initial Catalog=DotNet8Authentication;Persist Security Info=False;User ID=admin123;Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/16922b1a-ae0e-41e9-bc2b-c62836d225c4)

**IMPORTANT NOTE**: do not to forget to replace your password in the above connectionstring

we replace **{your_password}** with **Luiscoco123456**

This is the final connection string

```
Server=tcp:myidentityserver.database.windows.net,1433;Initial Catalog=DotNet8Authentication;Persist Security Info=False;User ID=admin123;Password=Luiscoco123456;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

We also have to **Configure network access** to your SQL server

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/515f31f3-f634-471c-9c70-750e615a1a48)

We add our local laptop IP address in the firewall rules

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/c267c8fd-ec8f-48e2-94ad-11d2ee31d2c3)

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/cb71ab21-e348-4169-80be-554ba22b80a5)

We run SSMS and check the Azure SQL database is working fine

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/9aef64d9-bf89-4228-98e9-57fb9d133d41)


## 2. Create WebAPI in Visual Studio 2022 Community Edition

We run Visual Studio 2022 and create a new project

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/af80c3d0-14d1-4e5c-b245-9e34425d6ab9)

We select the project template 

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/8877f4cf-2812-4fa8-8b0e-de34f31b3634)

We input the project name and the project location

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/e08ccea7-1e64-4fb9-a9a4-06b1724af18c)

We leave the default project features and we also select/check **Use Controllers**

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/e1ff147f-5b14-474b-9bf2-3d5e845a93df)

This is the default project structure we create

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/5b8f7ff7-d5c5-46b9-afa1-cfac3d4494ef)

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

This first command is to install EntityFramework

```
dotnet tool install --global dotnet-ef
```

This command is to check the EntityFramework version already installed

```
dotnet ef
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/08f17aa7-1ba2-4710-bd21-091758bf786c)

Now we create the initial migration

```
dotnet ef migrations add initial
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/fdecfed6-b884-4770-aed3-4f79417fcf75)

Also the Migrations folder will be created

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/8b29a4b4-9bdf-4e07-b895-91bd1f97c013)

Now we have to set a new vairable in the csproj

## IMPORTANT NOTE

In the project file set the variable "InvariantGlobalization" to "false"

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/c1876399-d033-4476-8ffd-2b18303a7246)

We also update the database

```
dotnet ef database update
```

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/0f71be5b-516a-4c6e-86ef-2a8ee0e4874d)

We check in SSMS the new tables created inside the database

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/e9337be1-0758-4e8c-8cec-bffec640f6a2)

## 8. How to test the application

Build and run the application with Visual Studio 2022

![image](https://github.com/luiscoco/Identity_dotNET8_Authentication/assets/32194879/e5107329-3049-4341-ba6a-8238011024eb)

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



