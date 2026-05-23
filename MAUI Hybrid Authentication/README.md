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
