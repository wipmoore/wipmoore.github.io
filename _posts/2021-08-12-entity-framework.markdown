---
layout: post
title: "EntityFramework Core"
date: 2021-08-12 15:00:00 +0100
categories:
---
- [DbContext](#dbcontext)


## DbContext

Represents a session within the database and is used to query and save instances of entities.  It is a combination of the __Unit Of Work__ and __Repository__ patterns.

- [DbContext](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-5.0)


The use of a DbContext typically involves:

- Creation of a DbContext instance
- Tracking entity instances.  Entities become tracked when:
  - Returned from a query.
  - Added or attached to the context
- Changes are made to the tracked entities
- Changes are written back to the database via __SaveChanges__ or __SaveChangesAsync__


```c#

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}

```


```c#

public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    services.AddDbContext<ApplicationDbContext>(
        options => options.UseSqlServer("name=ConnectionStrings:DefaultConnection"));
}

```

__N__. Entity framework core does not support multiple parallel operations being run on the same DbContext instance.


### Configuration

The default way to supply the connection string is via configuration:

e.g. for SQL server

```json
{
	...
	
	"ConnectionStrings": {
		"BooksContext": "Server=MyServer;Database=MyDb;Trusted_Connection=True;"
	},
	...
}
```

```c#
public void ConfigureServices(IServiceCollection services)
{
	services.AddDbContext<ConfigurationContext>(options => {
		options.UseSqlServer(Configuration.GetConnectionString("MyConnection"));
    });
}
```


e.g. for Postgress

```json
{
	...
	"ConnectionStrings": {
		"BooksContext": "Host=localhost;Username=postgres;Password=root;Database=asp_trial_api;Pooling=true;"
	},
	...
}
```

```c#
public void ConfigureServices(IServiceCollection services)
{

    services.AddDbContext<BloggingContext>(options =>
            options.UseNpgsql(Configuration.GetConnectionString("BooksContext")));
}
```

The DbContext has an __OnConfigureing__ method that can be overriden to set additional configuration ( usually around logging etc ).

```c#
protected override void OnConfiguring( DbContextOptionsBuilder optionsBuilder )
{
	...
}
```

