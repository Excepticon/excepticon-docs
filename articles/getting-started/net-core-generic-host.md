# Using Excepticon in a Generic Host

Adding Excepticon to your .NET Core generic host project can be accomplished in a few simple steps.

### 1. Install the Excepticon Nuget Package

Install the latest version of the **Excepticon** package in your .NET Core project via the Nuget Package Manager, the Package Manager Console, or the dotnet CLI.

Package Manager Console:

```Package Manager Console
Install-Package Excepticon
```

dotnet CLI:

```dotnet CLI
dotnet add package Excepticon
```



### 2. Add Excepticon When Creating the IHostBuilder

In Program.cs, add a call to `.AddExcepticon()` when configuring logging for your `IHostBuilder`:

```        csharp
static Task Main(string[] args) =>
    new HostBuilder()
        .ConfigureHostConfiguration(c =>
        {
            c.SetBasePath(Directory.GetCurrentDirectory());
            c.AddJsonFile("appsettings.json");
        })
        .ConfigureServices((c, s) => s.AddHostedService<MyHostedService>())
        .ConfigureLogging((c, l) =>
        {
            l.AddConfiguration(c.Configuration);
            l.AddExcepticon();
        })
        .Build()
        .RunAsync();
```



### 3. Add Excepticon API Key to Configuration

The `AddExcepticon()` method has a few overloads.  When the overload with no parameters is used (as shown above), the Exception ApiKey will attempt to be retrieved from one of the registered .NET Core [configuration providers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-3.1).

In the example above, the `appsettings.json` configuration is registered, and the file looks like this:

```json
{
  "Excepticon": {
    "ApiKey": "{Your ApiKey Here}"
  } 
}
```

You project's API Key can be obtained from the Project Settings page for your project of the Excepticon web app.

The API Key can also be stored and retrieved from an environment variable, and app setting on the Azure app service, Azure key vault, or any other registered configuration provider, as long as it is nested in an `Excepticon` subsection.



### Alternatives

Though storing the Excepticon API Key as a properly secured configuration setting and accessing it via a configuration provider is the preferred method, there is an overload for `.AddExcepticon()` that lets you pass your project's ApiKey directly in code:

```csharp
l.AddExcepticon("{Your ApiKey Here");
```

This is an easy way to test integration with Excepticon, but is not recommended for production apps and services.




### Source Code

Source code showing the configuration of Excepticon in a .NET Core generic host project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.GenericHost).