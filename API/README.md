# .NET

## Setup

### Tools

Recommended:

- [Download .NET](https://dotnet.microsoft.com/fr-fr/download)
- [Download Node.js](https://github.com/nvm-sh/nvm) (with Node Version Manager)

For macos users:

- *auto-completion doesn't work on VSCode*,
- *Visual Studio is'nt supported anymore*,
- *so [Rider](https://www.jetbrains.com/fr-fr/rider/download/#section=mac) of JetBrains (non-commercial) seems the better choice*.

For windows users:

- [Visual studio community edition](https://visualstudio.microsoft.com/fr/downloads/)

### Test installation

```bash
dotnet --info
```

### List commands

```bash
dotnet -h
```

## Create API with CLI

Get help on a specific command:

```bash
dotnet new -h
```

List project types:

```bash
dotnet new list
```

Create a solution:

```bash
dotnet new sln
```

Create web api:

```bash
dotnet new webapi -controllers -n API
```

List projects in solution:

```bash
dotnet sln list
```

Add project to solution:

```bash
dotnet sln add API
```

Run API:

```bash
cd API
```

```bash
dotenv run
```

Test API:

```bash
curl http://localhost:5066/WeatherForecast
```

Tip: use jq to format curl json output:

On macOS:

```bash
brew install jq
````

```bash
curl -s http://localhost:5066/WeatherForecast | jq .
```

Update ports and https configuration:

API/Properties/launchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "http://localhost:5000;https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

Trust https certificates:

```bash
sudo dotnet dev-certs https --trust
```

If it didn`t work:

```bash
sudo dotnet dev-certs https --clean
```

or

(delete certificate file in directory `~/.aspnet/dev-certs/` in case of error on app restart)

and 

```bash
sudo dotnet dev-certs https --trust
```

Use watch command to enable hot reload:

```bash
dotnet watch
```

## Development

### Entity

```csharp
namespace API.Entities;

public class AppUser
{
    public int Id { get; set; }

    public required string UserName { get; set; }
    
}
```

### Entity Framework

Definition: it's an ORM

LINQ (Language-INtegrated Query)

Features: Querying, Change Tracking, Saving, Concurrency, Transactions, Caching, Built-in conventions Configurations, Migrations.

#### Installation

1. Open NuGet View

2. For SQlite database engine, search for "microsoft.entityframework.sqlite" and add it to API project.

3. For migrations, search for "microsoft.entityframework.design" and add it to API project.

   *select the same major version as .NET installed example: 8.0.12 for .NET 8*

### Adding DbContext

/API/Data/DbContext.cs

```csharp
using API.Entities;
using Microsoft.EntityFrameworkCore;

namespace API.Data;

public class DataContext(DbContextOptions options) : DbContext(options)
{
    public DbSet<AppUser> Users { get; set; }
} 
```

### Install dotnet-ef CLI

```bash
dotnet tool update dotnet-ef --version 8.0.12 --global
```

### Init Migrations

In API project:

```bash
dotnet ef migrations add InititalCreate -o Data/Migrations
```

### Create the database using the first migrations

```bash
dotnet ef database update
```

## Http Request

### Allow CORS

Example for Angular dev: "localhost:4200"

Program.cs
```csharp
...
// builder
builder.Services.AddCors();
...
// middleware
app.UseCors(x => x.AllowAnyHeader().AllowAnyMethod().WithOrigins("http://localhost:4200","https://localhost:4200"));
...
```

### Adding HTTPS to Angular with mkcert

At root of angular project:

```bash
mkdir ssl && cd ssl 
```

Create a local certificate authority:

```bash
mkcert -install
```

Create a certificate for localhost:

```bash
mkcert localhost
```

In angular.json add ssl, key and certificate:

```json
{
  "serve": {
    "options": {
      "ssl": true,
      "sslKey": "ssl/localhost-key.pem",
      "sslCert": "ssl/localhost.pem"
    }
  }
}
```

### Update UserEntity

```csharp
...
public required byte[] PasswordHash { get; set; }
    
public required byte[] PasswordSalt { get; set; }
...    
```

```bash
dotnet ef migrations add UserEntityUpdated
```

```bash
 dotnet ef database update
```

### Use DTOs

Allow to limit data visibility of Database
Lighter data transfer in application layers

### Use the debbuger

- Create configuration to attach process to debugger
- Add breakpoints on code to analyse,
- Run the debugger,
- Navigate step by step,
- Watch variables values, on mouseover in code or in variables view.







