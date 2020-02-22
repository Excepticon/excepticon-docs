# Basic Usage

For many project types, adding Excepticon error monitoring can be accomplished in just a couple of simple steps.

### 1. Install the Excepticon Nuget Package

Install the latest version of the **Excepticon** package in your project via the Nuget Package Manager, the Package Manager Console, or the dotnet CLI.

Package Manager Console:

```Package Manager Console
Install-Package Excepticon
```

dotnet CLI:

```dotnet CLI
dotnet add package Excepticon
```



### 2. Initialize the ExcepticonSdk

Initialize the `ExcepticonSdk` with your project's ApiKey:

```        csharp
static void Main(string[] args)
{
    using (ExcepticonSdk.Init("{Your ApiKey Here}"))
    {
        // Application logic
        
        // Test that exceptions are being sent to Excepticon
        throw new ApplicationException("My test exception.");
    }
}
```

Unhandled exceptions within the using block will automatically be sent to Excepticon.

You project's `ApiKey` can be obtained from the Project Settings page for your project of the Excepticon web app.



### Other Usage Scenarios

While the steps above will apply to many project types, some types of projects ([ASP.NET Core](asp-net-core.md), [.NET Core generic host](net-core-generic-host.md), [Azure Functions](azure-functions.md), etc...) will require a slightly different configuration.

Please visit the [Getting Started Documentation](index.md) to see the configurations for several different types of .NET projects.



### Source Code

Source code showing the configuration of Excepticon in a console applicaiton project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.Console).