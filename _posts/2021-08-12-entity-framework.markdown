---
layout: post
title: "EntityFramework Core"
date: 2021-08-12 15:00:00 +0100
categories:
---
- [DbContext](#dbcontext)
- [Models](#models)
- [Querying Data](#querying-data)


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

## Models

Q. What is the model?

An entity is included in the model in one of the following ways:

- Including a DbSet of a type on the Context includes that type in the Model.
- A entity can be added to the model via the __OnModelCreating__ method.
- Any types that are found by recursively exploring the navigation properites of other discovered entities are included in the model. 

The model is mapped to a database schema through a combination of:

- Convention
- Attributes ascribed to the Entity types
- Via the fluent API of the ModelBuilder class

The mapping has to descibe how the properites of the entities are mapped to the fields in the database and how the relations between the objects map 


### Relations 

By default a relation will be created when there is a navigation property discovered on a type.  A relationship may be fully defined where there is a navigation property and a foreign key defined in the dependent entity.

```c#
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

The relationship can be configured using the ModelBuilder:

```c#
internal class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```


__N__. If there is no foreign key property a _shadow foreign key property_ will be defined. Where a shadow property is a property that is not defined on the class but is defined in the model.

```c#
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    // Since there is no CLR property which holds the foreign
    // key for this relationship, a shadow property is created.
    public Blog Blog { get; set; }
}
```

Single navigation properties can be introduced:

```c#
internal class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasMany(b => b.Posts)
            .WithOne();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
}
```
## Querying Data

Entity Framework used Linq for query definitions and will attempt to evaluate queries on the server as much as possible.  By default information about the loaded entities is maintained in the Contexts change tracker so that any changes that are made to an entity will be persisted to the database when SaveChanges is called.  Change tracking for a query can be be disabled.

```c#

var blog = context.Blogs
	.AsNoTracking()
	.ToList();

// or

context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehaviour.NoTracking;

var blogs = context .Blogs.ToList();
```


### Loading related data

There are three strategies used for this:

- __Eager Loading__ where the data is loaded as part of the initial query.
- __Explicit loading__ the related data is explicitly loaded at a later time.
- __Lazy loading__ the related data is transparently loaded from the database when the navigation property is accessed.


#### Eager loading

The __Include__ and __ThenInclude__ methods are used to specify the data to be included in the query result.

```c#
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Include(blog => blog.Posts)
        .ThenInclude(post => post.Author)
        .ThenInclude(author => author.Photo)
        .Include(blog => blog.Owner)
        .ThenInclude(owner => owner.Photo)
        .ToList();
}
```

Include statement can be filtered:

```c#

using (var context = new BloggingContext())
{
    var filteredBlogs = context.Blogs
        .Include(
            blog => blog.Posts
                .Where(post => post.BlogId == 1)
                .OrderByDescending(post => post.Title)
                .Take(5))
        .ToList();
}

```


#### Explicit Loading 

Where dependent entities are loaded in a separate statements:

```c#
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);

    context.Entry(blog)
        .Collection(b => b.Posts)
        .Load();

    context.Entry(blog)
        .Reference(b => b.Owner)
        .Load();
}
```


#### Lazy Loading

This can be done in different ways bu the most straight forward way to enable lazy loading is using the __Micorsoft.EntityFrameworkCore.Proxy__ package.

```c#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);

```
