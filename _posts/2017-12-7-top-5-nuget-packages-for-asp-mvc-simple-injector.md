---
layout: post
title: Top 5 Nuget Packages for ASP MVC - Simple Injector
--- 

This is entry number 3 of the top 5 nuget packages for ASP MVC 5 applications.

Previously entries in this list include:

1. [Automapper](/top-5-nuget-packages-for-asp-mvc-automapper).
2. [Lambda Expression Helpers](/top-5-nuget-packages-for-asp-mvc-lambda-expression-helpers).

Next up is..

# Simple Injector
 
```
Install-Package SimpleInjector
```

## What does it do?

Simple injector is a dependancy injection container. 

## Why use it?

Using a dependancy injection container helps you to comply with the I and D of the SOLID design principles. 

There are a lot of dependancy injection containers out there:

- Catle Windsor
- Structure Map
- Autofac
- Unity
- Ninject

... to name but a few! Simple injector for me is hands down the best choice though for two reasons:

1. It's simple! It doesn't have some of the more complex features of the others but I know I havent had need of them!
2. It's the fastest. Compared with the others it's much quicker at resolving dependancies (google some benchmarks). 


## How to use it

Here is an example controller before using Simple Injector:

```
public BooksContoller : Controller
{
    private readonly IBooksService _booksService;

    public ApplesContoller()
    {
        _booksService = new Bookservice();    
    }
}
```

Here is an example controller after using Simple Injector:

```
public BooksContoller : Controller
{
    private readonly IBooksService _booksService;

    public ApplesContoller(IBooksService booksService)
    {
        _booksService = booksService;    
    }

}
```

As you can see with the first example there is a hard dependancy on "Bookservice". You end up with tightly coupled classes breaking SOLID principles. The second example shows how the controller is passed a class that implements IBookService. A class that implements IBookService is defined in the Simple Injector configuration and that class will be used. The class doesnt know what that class will be just that it will conform to the promises the implementation promises thatthe interface defines.

## Tips 

### 1. Assembly scanning is your friend

If you have a huge project registering every single dependancy can get really tedious really fast: 

```
container.Register<ISomeRepository, SomeRepository>();
container.Register<IAnotherRepository, AnotherRepository>();
container.Register<ISomeService, SomeService>();
container.Register<IAnotherService, AnotherService>();
```


Intead you can scan assemblies and auto register implementations  :

```
var myAssembly = typeof(someclass).Assembly;

var registrations =
    from type in myAssembly.GetExportedTypes()
    where type.GetInterfaces().Any()
    select new { Service = type.GetInterfaces().Single(), Implementation = type };

foreach (var reg in registrations) {
    container.Register(reg.Service, reg.Implementation, Lifestyle.Transient);
}
```

## 2. Start-up Actions


If you want a bunch of things to run on application start up you can create an Interface such as:

```
    public interface IStartUpAction
    {
        void Execute();
    }
```

You can then register all classes that implement that interface:

```
    container.Collection.Register(typeof(IStartUpAction), localAssemblies);
```

And then in startup execute all the classes that implement the given interface.

```
    var startUpTasks = container.GetAllInstances<IStartUpAction>();

    foreach(var startUpTask in startUpTasks)
    {
        startUpTask.Execute();
    }
```

# Next Time

Next up I'm going to be looking at the Foolproof package and why it's awesome. Stay tuned!