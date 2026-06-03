In newer .NET MAUI templates (especially .NET 8, .NET 9, and later Visual Studio 2026 templates), **AppShell.xaml may not be created automatically** depending on the project type you selected.

### Why AppShell.xaml is Missing

#### 1. Blazor Hybrid Template

If you created a **.NET MAUI Blazor Hybrid App**, the navigation is handled by Blazor's router (`Routes.razor` / `Router`) instead of Shell navigation.

Typical structure:

```text
MauiProgram.cs
App.xaml
App.xaml.cs
Components/
    Layout/
    Pages/
    Routes.razor
```

`AppShell.xaml` is not required.

---

#### 2. Navigation-Based MAUI Template

Some templates use:

```csharp
public App()
{
    InitializeComponent();

    MainPage = new NavigationPage(new MainPage());
}
```

instead of:

```csharp
MainPage = new AppShell();
```

In this case no Shell file is generated.

---

#### 3. Template Changes in Visual Studio 2026

Microsoft simplified several MAUI templates. Depending on the selected template:

| Template                    | AppShell.xaml |
| --------------------------- | ------------- |
| .NET MAUI App               | Usually Yes   |
| .NET MAUI Blazor Hybrid App | No            |
| MAUI Empty App              | No            |
| Custom Templates            | Depends       |

---

### How to Add AppShell Manually

Create **AppShell.xaml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Shell
    x:Class="MyApp.AppShell"
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:views="clr-namespace:MyApp.Views">

    <ShellContent
        Title="Home"
        ContentTemplate="{DataTemplate views:MainPage}" />
</Shell>
```

Create **AppShell.xaml.cs**

```csharp
namespace MyApp;

public partial class AppShell : Shell
{
    public AppShell()
    {
        InitializeComponent();
    }
}
```

Update **App.xaml.cs**

```csharp
public App()
{
    InitializeComponent();

    MainPage = new AppShell();
}
```

---

### Check Your Project Type

Look at your `.csproj` file:

```xml
<UseMaui>true</UseMaui>
```

If you also see:

```xml
<PackageReference Include="Microsoft.AspNetCore.Components.WebView.Maui" />
```

then it's a **Blazor Hybrid** project and you normally use `Routes.razor` rather than `AppShell.xaml`.

If you paste your `.csproj` file or your project folder structure, I can tell you exactly why `AppShell.xaml` is missing and whether you should use Shell navigation or Blazor routing.
