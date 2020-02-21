# Using Excepticon in a WPF Application

Adding Excepticon to your WPF application project can be accomplished in a couple simple steps.

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

In App.xaml.cs, intialize the `ExcepticonSdk` with you project's ApiKey:

Example:

```        csharp
public partial class App : Application
{
    private IDisposable _excepticonSdk;

    private void Application_Startup(object sender, StartupEventArgs e)
    {
        _excepticonSdk = ExcepticonSdk.Init("{Your ApiKey Here}");
        MainWindow wnd = new MainWindow();
        wnd.Show();
    }

    protected override void OnExit(ExitEventArgs e)
    {
        _excepticonSdk.Dispose();

        base.OnExit(e);
    }
}
```

Replace the `StartupUri` attribute in your App.xaml with the `Startup` attribute pointing to you startup method:

```xaml
<Application x:Class="Excepticon.Examples.Wpf.App"
             ...
             Startup="Application_Startup">
```

Unhandled exceptions thrown during the lifetime of the application will automatically be sent to Excepticon.

You project's `ApiKey` can be obtained from the Project Settings page for your project of the Excepticon web app.



### Source Code

Source code showing the configuration of Excepticon in a WPF applicaiton project can be found [here](https://github.com/Excepticon/excepticon-dotnet/tree/master/examples/Excepticon.Examples.Wpf).