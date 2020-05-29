---
layout: post
title: Modular Blazor
--- 


# Modular Blazor

Blazor is great. It's the new shiny. It enables the creation of client web apps by people who are good with C# and HTML. It's for people who don't want to have to learn Angular, React *[insert latest JavaScript SPA framework or preference here]* to create modern user friendly responsive interfaces, but use what they know. 

Read up all about it [On Microsofts Site]([https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)

## Why Go Modular?

If a web app is modular is has a number of benefits:

- Plug-in like architecture (use what you need scrap what you don't)
- Follows the single responsibility principle of SOLID (less brittle code)
- Teams can work on parts of a whole in isolation (less merge issue headaches)

## How do you go Modular with Blazor?

In short: **Razor Class Libraries** and the **AdditionalAssemblies** on the Blazor router.

### Razor Class Libraries

A Razor Class Libraries allow you to create a project which contains

- Static Assets (css, js etc)
- Components
- Blazor Pages

Which is exactly what you would want in a Blazor Module. So how do you get that module to load as a module?

### Blazor router (AdditionalAssemblies)

The router has a baked in parameter that takes an array of additional assemblies to look at and find any additional routes. Cleverly named AdditionalAssemblies

So you can use the "AdditionalAssemblies" parameter to point to a method which scans the referenced Razor Class Libraries for Pages.

```
<Router AppAssembly="@typeof(Program).Assembly" AdditionalAssemblies="AssemblyScanning.GetAssemblies().ToArray()">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

## Show me the code (give me a demo to play with)

I have set up an example with two test modules on github which you can download/fork: 

https://github.com/treefishuk/ModularBlazor

If you are not a fan of the Assembly scanning code an alternative would be Scrutor: 

[https://github.com/khellang/Scrutor](https://github.com/khellang/Scrutor)
