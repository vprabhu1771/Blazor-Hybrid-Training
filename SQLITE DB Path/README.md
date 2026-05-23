# sqlite-net-pcl
```
https://www.nuget.org/packages/sqlite-net-pcl/1.9.172
```

```
MauiProgram.cs
```

```
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
            builder.Services.AddSingleton(serviceProvider => {
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


```
Pages\Home.razor
```

```
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.
@dbPath

@code {
	private string dbPath = Path.Combine(FileSystem.AppDataDirectory, "app.db");
}
```
