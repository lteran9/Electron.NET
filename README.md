# Electron.NET
Empty C# .NET Core 6.0 (MVC) application that uses ElectronNET to build a cross-platform graphical user interface (GUI).

## Requirements
- [.NET 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
- [Node](https://nodejs.org/en/download/)
- [NPM](https://sourceforge.net/projects/npm.mirror/) 
- [ElectronNET API](https://www.nuget.org/packages/ElectronNET.API/)
- [ElectronNET CLI](https://www.nuget.org/packages/ElectronNET.CLI/)

## Instructions
Once you have all the requirements installed on your OS you'll want to create a new .NET Core project via the CLI.

```
dotnet new mvc -o <ProjectName>
```

Afterwards, you'll have to add the ElectronNET.API NuGet package. The best way to do this is to also add a nuget.config file to your project.

```
dotnet new nugetconfig
```
```
dotnet add package ElectronNET.API
```

Once this is done go ahead and open the Program.cs file inside your project. There are a few lines you'll want to add to make use of Electron within your application. First off, add the using statement to the beginning of the file.

```
using ElectronNET.API;
```

Next, you'll have to tell the WebHost to use Electron by adding the below line after the builder is created.

```
builder.WebHost.UseElectron(args);
```

Lastly, add the below code before the app.Run() statement.

```
if (HybridSupport.IsElectronActive)
{
   CreateElectronWindow();
}
```

The compiler will complain that there is no CreateElectronWindow() method so let's go ahead and creat it at the end of the file.

```
async void CreateElectronWindow()
{
   var window = await ElectronNET.API.Electron.WindowManager.CreateWindowAsync();
   window.OnClose += () => ElectronNET.API.Electron.App.Quit();
}
```

Once the changes to Program.cs are complete we will install the Electron.CLI tool that basically runs the application in the target OS.

```
dotnet tool install --global Electron.CLI
```

You can then run the application with the below commands.

```
electronize init
electronize start
```