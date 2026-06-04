To enable **CommunityToolkit.Maui** in a .NET MAUI application, follow these steps.

### 1. Install the NuGet Package

Install the official package:

[CommunityToolkit.Maui NuGet Package](https://www.nuget.org/packages/CommunityToolkit.Maui/?utm_source=chatgpt.com)

Using .NET CLI:

```bash
dotnet add package CommunityToolkit.Maui
```

Or use Visual Studio:

* Right Click Project
* Manage NuGet Packages
* Search: `CommunityToolkit.Maui`
* Install

---

### 2. Register CommunityToolkit in `MauiProgram.cs`

```csharp
using CommunityToolkit.Maui;

namespace MauiApp1;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();

        builder
            .UseMauiApp<App>()
            .UseMauiCommunityToolkit()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        return builder.Build();
    }
}
```

---

### 3. Use Toolkit Controls

Example: Popup

```csharp
using CommunityToolkit.Maui.Views;

public partial class MyPopup : Popup
{
    public MyPopup()
    {
        InitializeComponent();
    }
}
```

Show popup:

```csharp
await this.ShowPopupAsync(new MyPopup());
```

---

### 4. Use Toolkit Converters, Behaviors, and Animations

Example Behavior:

```xml
<Entry>
    <Entry.Behaviors>
        <toolkit:EmailValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Add namespace:

```xml
xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"
```

---

### 5. Verify Installation

Build and run the project. If the following line compiles successfully, the toolkit is enabled:

```csharp
.UseMauiCommunityToolkit()
```

### Typical `MauiProgram.cs` for MAUI Blazor Hybrid

```csharp
using CommunityToolkit.Maui;
using Microsoft.AspNetCore.Components.WebView.Maui;

namespace MauiApp1;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();

        builder
            .UseMauiApp<App>()
            .UseMauiCommunityToolkit()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        builder.Services.AddMauiBlazorWebView();

#if DEBUG
        builder.Services.AddBlazorWebViewDeveloperTools();
#endif

        return builder.Build();
    }
}
```

If you're using **.NET MAUI Blazor Hybrid**, I can also show how to use CommunityToolkit **Toast**, **Snackbar**, **Popup**, and **Camera** features from Blazor pages.
