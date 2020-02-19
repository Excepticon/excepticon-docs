# Using Excepticon in ASP.NET Core

Adding Excepticon to your ASP.NET Core project can be accomplished in a few simple steps.

### 1. Install the Excepticon.AspNetCore Nuget Package

Install the latest version of the **Excepticon.AspNetCore** package in your ASP.NET Core project via the Nuget Package Manager, the Package Manager Console, or the dotnet CLI.

Package Manager Console:

```Package Manager Console
Install-Package Excepticon.AspNetCore
```

dotnet CLI:

```dotnet CLI
dotnet add package Excepticon.AspNetCore
```



### 2. Add Excepticon When Creating the IWebHostBuilder

In Program.cs, add a call to `.UseExcepticon()` when building  your `IWebHostBuilder`.

Example:

```        csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration(config =>
        {
            config.AddJsonFile("local.settings.json", true, true);
        })
        .UseStartup<Startup>()
        .UseExcepticon();
```



### 3. Add Excepticon ApiKey to Configuration

The `UseExcepticon()` method has a few overloads.  When the overload with no parameters is used (as shown above), the Exception ApiKey will attempt to be retrieved from one of the registered .NET Core [configuration providers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-3.1).

In the example above, the `local.settings.json` configuration is registered, and the file looks like this:

```json
{
  "Excepticon": {
    "ApiKey": "{Your ApiKey Here}"
  } 
}
```

You project's `ApiKey` can be obtained from the Project Settings page for your project of the Excepticon web app.

The `ApiKey` can also be stored and retrieved from an environment variable, and app setting on the Azure app service, Azure key vault, or any other registered configuration provider, as long as it is nested in an `Excepticon` subsection.



### Alternatives

Though storing the Excepticon ApiKey as a properly secured configuration setting and accessing it via a configuration provider is the preferred method, there is an overload for `.UseExcepticon()` that lets you pass your project's ApiKey directly in code:

```csharp
.UseExcepticon("{Your ApiKey Here}");
```

This is an easy way to test integration with Excepticon, but is not recommended for production apps and services.




### Source Code

Source code showing the configuration of Excepticon in an ASP.NET Core project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.AspNetCore).