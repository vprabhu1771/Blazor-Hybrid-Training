#  MAUI Hybrid Authentication Made EASY for Beginners!
```
https://www.youtube.com/watch?v=Vogg3s54n0A
```

```
https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization
```

```
https://www.nuget.org/packages/sqlite-net-pcl
```

# Folder Setup
```
Models
Authentication
Repositories
Entites
```

# File Setup
```
Models -> UserSession.cs
Authentication -> AuthenticationService.cs
Authentication -> CustomAuthenticationStateProvider.cs
Repositories -> UserAccountRepository.cs
Entites -> UserAccount.cs
```

# 
```
Repositories -> UserAccountRepository.cs
```

```
using MauiApp1.Models;
using SQLite;

namespace MauiApp1.Repositories
{
    public class UserAccountRepository
    {
        private readonly SQLiteAsyncConnection _database;

        public UserAccountRepository(SQLiteAsyncConnection database)
        {
            _database = database;

            // Create table
            _database.CreateTableAsync<UserAccount>().Wait();
        }

        // Register User
        public async Task<int> AddUserAsync(UserAccount user)
        {
            return await _database.InsertAsync(user);
        }

        // Login User
        public async Task<UserAccount> GetUserAsync(string username, string password)
        {
            return await _database.Table<UserAccount>()
                                  .Where(x => x.UserName == username &&
                                              x.Password == password)
                                  .FirstOrDefaultAsync();
        }

        // Check Existing User
        public async Task<UserAccount> GetUserByUsernameAsync(string username)
        {
            return await _database.Table<UserAccount>()
                                  .Where(x => x.UserName == username)
                                  .FirstOrDefaultAsync();
        }

        // Get All Users
        public async Task<List<UserAccount>> GetAllUsersAsync()
        {
            return await _database.Table<UserAccount>().ToListAsync();
        }

        // Delete User
        public async Task<int> DeleteUserAsync(UserAccount user)
        {
            return await _database.DeleteAsync(user);
        }

        // Update User
        public async Task<int> UpdateUserAsync(UserAccount user)
        {
            return await _database.UpdateAsync(user);
        }
    }
}
```

# 
```
Entites -> UserAccount.cs
```
```
using SQLite;

namespace MauiApp1.Models
{
    public class UserAccount
    {
        [PrimaryKey, AutoIncrement]
        public int Id { get; set; }

        public string UserName { get; set; }

        public string Password { get; set; }

        public string Role { get; set; }
    }
}
```

# User Session
```
Models -> UserSession.cs
```

```
using System;
using System.Collections.Generic;
using System.Text;

namespace MauiApp1.Models
{
    public class UserSession
    {
        public string? UserName { get; set; }
        public string? Role { get; set; }

        public UserSession(string? userName, string? role)
        {
            UserName = userName;
            Role = role;
        }
    }
}
```


# Authentication Service
```
Authentication -> AuthenticationService.cs
```

```
using MauiApp1.Models;
using System;
using System.Collections.Generic;
using System.Text;
using System.Text.Json;

namespace MauiApp1.Authentication
{
    public class AuthenticationService
    {
        private const string USER_SESSION_KEY = "app_user_session";

        public async Task<UserSession> GetUserSession()
        {
            UserSession? userSession = null;

            var userSessionJson = await SecureStorage.Default.GetAsync(USER_SESSION_KEY);

            if (!string.IsNullOrWhiteSpace(userSessionJson))
            {
                userSession = JsonSerializer.Deserialize<UserSession>(userSessionJson);
            }

            return userSession;
        }

        public async Task SaveUserSession(UserSession userSession)
        {
            var userSessionJson = JsonSerializer.Serialize(userSession);
            await SecureStorage.Default.SetAsync(USER_SESSION_KEY, userSessionJson);
        }

        public async Task RemoveUserSession()
        {
            SecureStorage.Default.Remove(USER_SESSION_KEY);
        }
    }
}
```

# Routes Component
```
Components\Routes.razor
```

```
@using Microsoft.AspNetCore.Components.Authorization
<Router AppAssembly="typeof(MauiProgram).Assembly" NotFoundPage="typeof(Pages.NotFound)">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="routeData" DefaultLayout="typeof(Layout.MainLayout)">
            <NotAuthorized>
                @if (!context.User.Identity.IsAuthenticated)
                {
                    <RedirectToLogin />
                }
                else
                {
                    <p>You do not have the necessary permissions to access this page.</p>
				}				
            </NotAuthorized>
        </AuthorizeRouteView>
        <FocusOnNavigate RouteData="routeData" Selector="h1" />
    </Found>
</Router>
```

# Dependency Injection
```
using MauiApp1.Authentication;
using MauiApp1.Repositories;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.Logging;
using SQLite;

namespace MauiApp1
{
    public static class MauiProgram
    {
        public static MauiApp CreateMauiApp()
        {
            var builder = MauiApp.CreateBuilder();
            builder
                .UseMauiApp<App>()
                .ConfigureFonts(fonts =>
                {
                    fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
                });

            builder.Services.AddMauiBlazorWebView();

            builder.Services.AddSingleton<AuthenticationService>();
            builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthenticationStateProvider>();
            builder.Services.AddAuthorizationCore();

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
