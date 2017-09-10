---
layout: post
title: Top 10 Nuget Packages for ASP MVC - AutoMapper
--- 

Over the past few years building ASP MVC solutions there have been moments when I thought...

> "How did I not know about this!!!??"  

So I am doing a series on nuget packages for ASP MVC 5 solutions that I wish I had known about long ago that would have made my life a lot easier. 

First up is...


# Automapper
 
```

Install-Package AutoMapper

```

## What does it do?

Automapper maps one classes properties to another classes properties.

## Why use it?

Using Automaper helps you follow the DRY (don't repeat yourself) principle and the S in SOLID principles.

## How to use it

Here is an example controller before using automapper:

```
public BooksContoller : Controller
{
    private readonly IBooksService _booksService;

    public ApplesContoller(IBooksService booksService)
    {
        _booksService = booksService;    
    }


    public ActionResult Details(int? id)
    {
    
        if (id == null)
        {
            throw new HttpException(404, "not found");
        }
                
        var dto = _booksService.GetById(Id);
        
        BookViewModel viewModel = new BookViewModel {
            Id = dto.Id
            Title = dto.Title,
            ISBN = dto.ISBN
        };

        return View(viewModel);

    }

    public ActionResult Edit(int? id)
    {
    
        if (id == null)
        {
            throw new HttpException(404, "not found");
        }
                
        var dto = _booksService.GetById(Id);
        
        BookViewModel viewModel = new BookViewModel {
            Id = dto.Id
            Title = dto.Title,
            ISBN = dto.ISBN
        };

        return View(viewModel);

    }

}
```

As you can see the mapping of the DTO to the view model is repeated in the Details and Edit Method. This breaks the DRY principle. It also breaks the single responsibility principle becuase if both views now need and extra property like the release date added to the view model then the code will need changing in two places. 

Here is an example controller after using Automaper:


```
public BooksContoller : Controller
{
    private readonly IBooksService _booksService;

    public ApplesContoller(IBooksService booksService)
    {
        _booksService = booksService;    
    }


    public ActionResult Details(int? id)
    {
    
        if (id == null)
        {
            throw new HttpException(404, "not found");
        }
                
        var dto = _booksService.GetById(Id);
        
        BookViewModel viewModel = Mapper.Map<BookViewModel>(dto);

        return View(viewModel);

    }

    public ActionResult Edit(int? id)
    {
    
        if (id == null)
        {
            throw new HttpException(404, "not found");
        }
                
        var dto = _booksService.GetById(Id);
        
        BookViewModel viewModel = Mapper.Map<BookViewModel>(dto);

        return View(viewModel);

    }


}

```

As you can see there is no repeated manual mapping. Much tider methods, and just all round better.


## Tips 

### 1. Use Queryable extensions

A great use for automapper is using it to grab your domain entities from the database and map them to dtos. 

**When you are working with EntityFramework what ever you do don't use "Mapper.Map" in your mapping from domain entities to dtos.**

Say you have a book entity and that book entity has a associated author entity and you have the following mapping:

```
    CreateMap<Book, BookDto>()
            .ForMember(d => d.Id, x => x.MapFrom(e => e.Id))
            .ForMember(d => d.Title, x => x.MapFrom(e => e.Title))
            .ForMember(d => d.AuthorName, x => x.MapFrom(e => e.Author.Fullname))
            ;
```


You could do :

```

   public BooksService : IBooksService
   {

        private readonly IBookRepository _bookRepository;

        public BookService(IBookRepository bookRepository)
        {
            _bookRepository = bookRepository;
        }

        public BookDto GetBookById(int id)
        {
            var entity = _bookRepository.GetById(id);

            return Mapper.Map<BookDto>(entity);

        }

   }


```

But doing so would likley grab the entire book entity from the database when all that is needed are a few properties that exist on the BookDto.

Even worse is the Authors name, becuase it would either be grabed via lazy loading making an additional select statement or by doing an include which would grab the entire Author entity as well when all you need is the Name.

So instead you can use automapper projections

```

   public BooksService : IBooksService
   {

        private readonly IBookRepository _bookRepository;

        public BookService(IBookRepository bookRepository)
        {
            _bookRepository = bookRepository;
        }

        public BookDto GetBookById(int id)
        {
            return _bookRepository.GetQueryable(id).projectTo<BookDto>().Single();

        }

   }


```

This will grab only the required data in a single Select statement and is the bigest thing you could do to improve the performance of Enity Framework! 


## 2.  Use profiles


If you use a single file for your mappings the file can get huge and messy very quickly. 

I would recomend a sinlge mapping file per entity.

For Example : 

```

namespace MyProject.Core.Mappings
{

    public class BookEntityMappings : Profile
    {

        public BookEntityMappings()
        {

            CreateMap<Book, BookDto>()
            .ForMember(d => d.Id, x => x.MapFrom(e => e.Id))
            .ForMember(d => d.Title, x => x.MapFrom(e => e.Title))
            .ForMember(d => d.AuthorName, x => x.MapFrom(e => e.Author.Fullname))
            ;

        }
    }
}

```

You can then on startup check each assembly for mapping files and add them all automatically.  

```

namespace AppStart
{
    public static class AutoMapperConfiguration
    {
        public static void Configure()
        {
            Mapper.Initialize(cfg =>
            {

                cfg.AddProfiles(new[] {
                    "MyProject.Core",
                    "MyProject.Services",
                    "MyProject.Web"
                });


            });



            Mapper.AssertConfigurationIsValid();
        }
    }
}


```


## 3. Abstract it

Something better then Automapper may come along in the future so I would recomend abstracting the Mapper.Map methods into an interface.

```

namespace MyProject.Common.Interfaces.Helpers
{
    public interface IClassMapper
    {

        TDestination Map<TSource, TDestination>(TSource source);

        TDestination Map<TDestination>(object source);

        void Map(object source, object destination);

    }
}


namespace MyProject.Common.Helpers
{
    public class Mapper : IClassMapper
    {
        public TDestination Map<TSource, TDestination>(TSource source)
        {
            return AutoMapper.Mapper.Map<TSource, TDestination>(source);
        }

        public TDestination Map<TDestination>(object source)
        {
            return AutoMapper.Mapper.Map<TDestination>(source);
        }

        public void Map(object source, object destination)
        {
            AutoMapper.Mapper.Map(source, destination);
        }

    }
}


```

You can the use dependancy injection to inject the Mapper class into your project. 

My current faverite is SimpleInjector. It's simple and more importantly quick. 


# Next Time

Next up I'm going to be looking at the ASP.NET-MVC-Lambda-Expression-Helpers package and why it's awesome. Stay tuned!