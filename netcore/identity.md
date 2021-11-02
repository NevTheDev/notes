# ASP.NET Core Identity

# Core lib
Microsoft.extensions.identity.core
microsoft.aspnetcore.identity


## Storage Packages
Microsoft.extensions.identity.stores
microsoft.aspnetcore.identity.entityframeworkcore



## Managers

UserManager<Tuser> 

provides access t

RoleManager<Tuser>


## Data Stores
IUserStore<>


## Add Authentication

```c#
public void ConfigureServices(IServiceCollection services)
{
    ...
    services
        .AddAuthentication("cookies")
        .AddCookie("cookies", options => { });
    ...
```