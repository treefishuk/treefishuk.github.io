---
layout: post
title: Top 5 Nuget Packages for ASP MVC - X.PagedList
--- 

The final entry in this list of nuget awesomness for ASP MVC 5 solutions is:

# X.PagedList MVC
 
```
Install-Package X.PagedList.Mvc
```

## What does it do?

It pages your data. 

## Why use it?

So you don't fetch ludicrous amounts of data and blow up your repositories, and instead return smaller sets of data in pageable format.

## How to use it


### Controller

```
public class ContactsController : Controller
{
    public ActionResult Index(int? page)
    {
        var pageNumber = page ?? 1;
        
        var model = new ContactsIndexViewModel{
            PageTitle = "Contacts",
            OnePageOfProducts = _ContactsService.GetContacts().ToPagedList(pageNumber, 25)
        }

        return View(model);
    }
}
```

### View
```
@using X.PagedList.Mvc; 

<h1>@Model.PageTitle</h1>
<ul>
    @foreach(var contact in Model.OnePageOfContacts){
        <li>@contact.FullName</li>
    }
</ul>

@Html.PagedListPager(Model.OnePageOfContacts, page => Url.Action("Index", new { page }) )
```

That's all there is to it!

### Honourable Mentions

There are a bunch of good packages that didn't make the top 5 and are worth checking out. So if you want to check out even more awesomeness check out:

- [EntityFramework.DynamicFilters](http://entityframework-dynamicfilters.net)
- [Z.EntityFramework.Plus.EF6](http://entityframework-plus.net)
- [Elmah.Mvc](https://elmah.github.io/a/mvc/)

# Previous Entries

1. [Automapper](/top-5-nuget-packages-for-asp-mvc-automapper).
2. [Lambda Expression Helpers](/top-5-nuget-packages-for-asp-mvc-lambda-expression-helpers).
3. [Simple Injector](/top-5-nuget-packages-for-asp-mvc-simple-injector)
4. [Foolproof](/top-5-nuget-packages-for-asp-foolproof)

# Thanks!

Thanks for checking out my top 5 nuget packages for ASP MVC 5.