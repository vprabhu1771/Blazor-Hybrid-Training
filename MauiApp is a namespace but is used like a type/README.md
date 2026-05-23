The issue is here:

```csharp
var builder = MauiApp.CreateBuilder();
```

Your project namespace is also named `MauiApp`, so C# gets confused between:

* Namespace: `MauiApp`
* Class: `Microsoft.Maui.Hosting.MauiApp`

---

## Fix

Change this:

```csharp
var builder = MauiApp.CreateBuilder();
```

to:

```csharp
var builder = Microsoft.Maui.Hosting.MauiApp.CreateBuilder();
```

---

## Correct `MauiProgram.cs`

```csharp
using MauiApp.Repositories;
using Microsoft.Extensions.Logging;
using SQLite;

namespace MauiApp
{
    public static class MauiProgram
    {
        public static Microsoft.Maui.Hosting.MauiApp CreateMauiApp()
        {
            var builder = Microsoft.Maui.Hosting.MauiApp.CreateBuilder();

            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            builder.Services.AddMauiBlazorWebView();

            // SQLITE
            builder.Services.AddSingleton(serviceProvider =>
            {
                var dbPath = Path.Combine(FileSystem.AppDataDirectory, "app.db");
                return new SQLiteAsyncConnection(dbPath);
            });

            builder.Services.AddSingleton<UserAccountRepository>();

#if DEBUG
            builder.Services.AddBlazorWebViewDeveloperTools();
            builder.Logging.AddDebug();
#endif

            return builder.Build();
        }
    }
}
```

---

## Better Alternative (Recommended)

Rename your project namespace from:

```text
MauiApp
```

to something unique like:

```text
HybridAuthApp
```

or

```text
MyMauiApp
```

This avoids future conflicts with:

```csharp
Microsoft.Maui.Hosting.MauiApp
```

because `MauiApp` is already a framework class name.
