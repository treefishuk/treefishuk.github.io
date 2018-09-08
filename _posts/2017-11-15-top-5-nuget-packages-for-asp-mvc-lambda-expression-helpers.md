---
layout: post
title: Top 5 Nuget Packages for ASP MVC - Simple Injector
--- 

Over the past few years building ASP MVC solutions there have been moments when I thought...

> "How did I not know about this!!!??"  

So I am doing a series on nuget packages for ASP MVC 5 solutions that I wish I had known about long ago that would have made my life a lot easier. 

Previously entries in this list include:

1. [Automapper](/top-5-nuget-packages-for-asp-mvc-automapper).

Next up is..

# Lambda Expression Helpers
 
```

Install-Package System.Web.Mvc.Expressions

```

## What does it do?

It stops you using magic brittle strings for things such as redirects. 

## Why use it?

Say you have the following controller: 

```
public NewsContoller : Controller
{
       public ActionResult Index()
       {
            return View();
       }
}
```

And the following in a view:
```
@Html.ActionLink("News", "Index", "News")
```

Now imagine that the client decides actually they want to user "blogs" rather then news. And so you change the news controller to:

```
public BlogsContoller : Controller
{
       public ActionResult Index()
       {
            return View();
       }
}
```

Everything complies no errors will be thrown but when the user clicks on the link in the view they are going to get a 404 error. 

## How to use it

Givent he example above instead of using:

```

@Html.ActionLink("News", "Index", "News")

```

You could use:

```
@Html.ActionLink<NewsController>("News", c => c.Index())

```

This way if the controller gets renamed or the requirements of the index method change and the views are compiled on build there will be build errors to help you catch these kinds of problems.


## Tips 

### 1. For Controllers as well as views

Doing a redirect after posting a form? Instead of using: 

```
    return RedirectToAction("Index", "Home");

```

You can use:
```

    return RedirectToAction<HomeController>(c => c.Index())

```

# Next Time

Next up I'm going to be looking at Simple Injector and why it's awesome. Stay tuned!