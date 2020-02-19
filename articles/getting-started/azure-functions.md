# Using Excepticon in Azure Functions

In a conventional web application, error handling is often handled globally in the application's middleware.  Azure Functions, however, do not currently give the developer the ability to define custom middleware for an Azure Functions application, so for this guide, we'll be configuring Excepticon at the function level.

Adding Excepticon to your Azure Function can be accomplished in a few simple steps.

### 1. Install the Excepticon Nuget Package

Install the latest version of the **Excepticon** package in your Azure Functions project via the Nuget Package Manager, the Package Manager Console, or the dotnet CLI.

Package Manager Console:

```Package Manager Console
Install-Package Excepticon
```

dotnet CLI:

```dotnet CLI
dotnet add package Excepticon
```



### 2. Add Excepticon to Your Function

In your Azure Function, initialize the ExcepticonSdk and use it to capture any exceptions that occur during the execution of your function:

Example:

```        csharp
[FunctionName("Function1")]
public static void Run([TimerTrigger("0 */5 * * * *", RunOnStartup = true)]TimerInfo myTimer, ILogger log)
{
    using (ExcepticonSdk.Init(_appSettings.ExcepticonOptions)
    {
        try
        {
            // Function logic
        }
        catch (Exception ex)
        {
            ExcepticonSdk.CaptureException(ex);
        }
    }
}
```



### 3. Add Excepticon ApiKey to Configuration

In this example, I pass an instance of `ExcepticonOptions` which contains my project's ApiKey to the `ExcepticonSdk.Init` method.  `ExcepticonOptions` is initialized on startup from the app's configuration, and it is registered as a property on the `FunctionsAppSettings` object, which is registered with the DI container and directed into the Azure Function:

FunctionsAppSettings:

```csharp
public interface IFunctionsAppSettings : IAppSettings
{
    ExcepticonOptions ExcepticonOptions { get; }
}

public class FunctionsAppSettings : IFunctionsAppSettings
{
    public FunctionsAppSettings(IConfiguration config)
    {
        ExcepticonOptions = config.GetSection("Excepticon").Get<ExcepticonOptions>();
    }

    public ExcepticonOptions ExcepticonOptions { get; }
}
```

Instantiation and registration of `FunctionsAppSettings` in Startup.cs:

```csharp
public override void Configure(IFunctionsHostBuilder builder)
{
    var appSettings = new FunctionsAppSettings(_configuration, _isDevelopment);
    builder.Services.AddSingleton((IFunctionsAppSettings)appSettings);
}
```

Injection of `FunctionsAppSettings` into Azure Function:

```csharp
public TestExceptionTimerFunction(IFunctionsAppSettings appSettings)
{
    _appSettings = appSettings;
}
```

The Exception ApiKey will attempt to be retrieved from one of the registered .NET Core [configuration providers](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-3.1).

You project's `ApiKey` can be obtained from the Project Settings page for your project of the Excepticon web app.

The `ApiKey` can also be stored and retrieved from an environment variable, and app setting on the Azure app service, Azure key vault, or any other registered configuration provider, as long as it is nested in an `Excepticon` subsection.



### Alternatives

Though storing the Excepticon ApiKey as a properly secured configuration setting and accessing it via a configuration provider is the preferred method, there is an overload for `ExcepticonSdk.Init()` that lets you pass your project's ApiKey directly in code:

```csharp
using (ExcepticonSdk.Init("{Your ApiKey Here"))
```

This is an easy way to test integration with Excepticon, but is not recommended for production apps and services.




### Source Code

Source code showing the configuration of Excepticon in and Azure Functions project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.AzureFunctions).