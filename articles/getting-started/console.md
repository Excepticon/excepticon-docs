# Using Excepticon in a Console Application

Adding Excepticon to your console application project can be accomplished in a couple simple steps.

### 1. Install the Excepticon Nuget Package

Install the latest version of the **Excepticon** package in your console application project via the Nuget Package Manager, the Package Manager Console, or the dotnet CLI.

Package Manager Console:

```Package Manager Console
Install-Package Excepticon
```

dotnet CLI:

```dotnet CLI
dotnet add package Excepticon
```



### 2. Initialize the ExcepticonSdk

In Program.cs, intialize the `ExcepticonSdk` with you project's API Key:

```        csharp
static void Main(string[] args)
{
    using (ExcepticonSdk.Init("{Your ApiKey Here}"))
    {
        // Application logic
    }
}
```

Unhandled exceptions within the using block will automatically be sent to Excepticon.

You project's `ApiKey` can be obtained from the Project Settings page for your project of the Excepticon web app.



### Source Code

Source code showing the configuration of Excepticon in a console applicaiton project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.Console).