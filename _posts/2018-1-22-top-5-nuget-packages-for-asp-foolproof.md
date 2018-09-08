---
layout: post
title: Top 5 Nuget Packages for ASP MVC - Foolproof
--- 

The 4th entry in the list of nuget awesomness for ASP MVC 5 solutions is:

# Foolproof
 
```
Install-Package foolproof
```

## What does it do?

It adds conditional validation through use of Data Annotations.

## Why use it?

Say you have the following class: 

```
public class MyEpicClass
{
    public string PhoneNumber { get; set; }

    public bool HasPhone { get; set; }
}
```

If HasPhone is true then PhoneNumber should be required. Out of the box there is no way to do that. That is where foolproof comes in. It enables you to do this:

```
public class MyEpicClass
{

    [RequiredIfTrue("HasPhone")]
    public string PhoneNumber { get; set; }

    public bool HasPhone { get; set; }
}
```

It works with the JQuery validation library and includes script files to extend the functionality of unobtrusive JQuery validation as well.

## How to use it

Simply pop the nesseccary data attribute on the property that needs slightly more complex validation.

For the full list of available attributes check out the docs at [https://archive.codeplex.com/?p=foolproof](https://archive.codeplex.com/?p=foolproof)


# Previous Entries

1. [Automapper](/top-5-nuget-packages-for-asp-mvc-automapper).
2. [Lambda Expression Helpers](/top-5-nuget-packages-for-asp-mvc-lambda-expression-helpers).
3. [Simple Injector](/top-5-nuget-packages-for-asp-mvc-simple-injector)

# Next Time

For the final entry I'm going to be looking at X.PagedList. Stay tuned!