---
layout: post
title: Using ASP.NET Core, Markdig, and RestSharp to build a Markdown API
categories: projects
---

Before we begin, make sure that you have the latest version of [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) installed. This guide was written using .NET Core version 3.1, but future versions will likely work as well.

## Setting Up A New ASP.NET Core API

Open a new terminal to an empty project directory and run:

``` powershell
dotnet new webapi
```

This will scaffold a new ASP.NET Core Web API project in your project directory.

To add a Nuget package reference to [Markdig](https://github.com/lunet-io/markdig), run:

``` powershell
dotnet add package Markdig
```

The latest Markdig version at the time of writing is `0.18.1`.

Do the same for [RestSharp](http://restsharp.org/):

``` powershell
dotnet add package RestSharp
```

The latest RestSharp version at the time of writing is `106.10.1`.

## Building the Controller

By default, the .NET CLI will create a `WeatherForecastController.cs`. We can leave this for now.

Create a new file alongside `WeatherForecastController.cs` called `MarkdownController.cs`.

First, add the controller's namespace, class header, and controller decorators:

``` c#
namespace markdown_api.Controllers
{
    using Microsoft.AspNetCore.Mvc;

    [ApiController]
    [Route("[controller]")]
    public class MarkdownController : ControllerBase
    {

    }
}
```

Add a constructor within the `MarkdownController` class with paramter `ILogger<MarkdownCotnroller> logger`. We will use the logger for logging informational messages about the actions our controller is performing. Also add `using Microsoft.Extensions.Logging;` to your list of using statements.

``` C#
using Microsoft.Extensions.Logging;

public class MarkdownController : ControllerBase
{
    private readonly ILogger<MarkdownController> _logger;

    public MarkdownController(ILogger<MarkdownController> logger)
    {
        _logger = logger;
    }
}
```

Now lets add our two controller endpoints that we will use for publishing and retrieving our markdown data. We will need to also add using `using System.Threading.Tasks;` to our list of using statments.

``` C#
using System.Threading.Tasks;

public class MarkdownController : ControllerBase
{
    ...

    [HttpGet("{id}")]
    public async Task<IActionResult> GetMarkdown(string id)
    {

    }

    [HttpPost]
    public async Task<IActionResult> PublishMarkdown(string fileUrl)
    {

    }
}
```
