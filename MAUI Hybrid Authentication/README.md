#  MAUI Hybrid Authentication Made EASY for Beginners!
```
https://www.youtube.com/watch?v=Vogg3s54n0A
```

```
https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization
```

# Folder Setup
```
Models
Authentication
```

# File Setup
```
Models -> UserSession.cs
Authentication -> AuthenticationService.cs
Authentication -> CustomAuthenticationStateProvider.cs
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
