For a .NET MAUI Blazor Hybrid app, you can connect to SQL Server using `Microsoft.Data.SqlClient` exactly like ASP.NET — but there are important limitations:

* ✅ Works on **Windows**
* ⚠️ Android/iOS cannot directly connect to SQL Server in production
* ✅ Recommended approach for mobile:

  * MAUI App → Web API → SQL Server

For local/offline mobile apps, use SQLite instead.

---

# 1. Install SQL Server Package

In your MAUI Blazor Hybrid project:

```bash
dotnet add package Microsoft.Data.SqlClient
```

Or via NuGet:

* `Microsoft.Data.SqlClient`

---

# 2. Create Model

## `Models/User.cs`

```csharp
namespace MauiApp1.Models;

public class User
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
}
```

---

# 3. Create SQL Service

## `Services/UserService.cs`

```csharp
using Microsoft.Data.SqlClient;
using MauiApp1.Models;

namespace MauiApp1.Services;

public class UserService
{
    private readonly string _connectionString =
        "Data Source=SERVERNAME;" +
        "Initial Catalog=YourDatabase;" +
        "User ID=sa;" +
        "Password=yourpassword;" +
        "TrustServerCertificate=True";

    // READ
    public async Task<List<User>> GetUsersAsync()
    {
        List<User> users = new();

        using SqlConnection conn = new(_connectionString);

        await conn.OpenAsync();

        string query = "SELECT Id, Name FROM Users";

        using SqlCommand cmd = new(query, conn);

        using SqlDataReader reader = await cmd.ExecuteReaderAsync();

        while (await reader.ReadAsync())
        {
            users.Add(new User
            {
                Id = reader.GetInt32(0),
                Name = reader.GetString(1)
            });
        }

        return users;
    }

    // INSERT
    public async Task AddUserAsync(string name)
    {
        using SqlConnection conn = new(_connectionString);

        await conn.OpenAsync();

        string query = "INSERT INTO Users(Name) VALUES(@Name)";

        using SqlCommand cmd = new(query, conn);

        cmd.Parameters.AddWithValue("@Name", name);

        await cmd.ExecuteNonQueryAsync();
    }

    // UPDATE
    public async Task UpdateUserAsync(int id, string name)
    {
        using SqlConnection conn = new(_connectionString);

        await conn.OpenAsync();

        string query = "UPDATE Users SET Name=@Name WHERE Id=@Id";

        using SqlCommand cmd = new(query, conn);

        cmd.Parameters.AddWithValue("@Id", id);
        cmd.Parameters.AddWithValue("@Name", name);

        await cmd.ExecuteNonQueryAsync();
    }

    // DELETE
    public async Task DeleteUserAsync(int id)
    {
        using SqlConnection conn = new(_connectionString);

        await conn.OpenAsync();

        string query = "DELETE FROM Users WHERE Id=@Id";

        using SqlCommand cmd = new(query, conn);

        cmd.Parameters.AddWithValue("@Id", id);

        await cmd.ExecuteNonQueryAsync();
    }
}
```

---

# 4. Register Service

## `MauiProgram.cs`

```csharp
using MauiApp1.Services;

builder.Services.AddSingleton<UserService>();
```

---

# 5. Blazor CRUD Page

## `Pages/Users.razor`

```razor
@page "/users"

@using MauiApp1.Models
@using MauiApp1.Services

@inject UserService UserService

<h3>Users</h3>

<input @bind="newUserName" placeholder="Enter Name" />
<button @onclick="AddUser">Add</button>

<hr />

<table class="table">
    <thead>
        <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Actions</th>
        </tr>
    </thead>

    <tbody>
        @foreach (var user in users)
        {
            <tr>
                <td>@user.Id</td>

                <td>
                    @if (editId == user.Id)
                    {
                        <input @bind="editName" />
                    }
                    else
                    {
                        @user.Name
                    }
                </td>

                <td>
                    @if (editId == user.Id)
                    {
                        <button @onclick="SaveUser">Save</button>
                    }
                    else
                    {
                        <button @onclick="() => EditUser(user)">Edit</button>
                    }

                    <button @onclick="() => DeleteUser(user.Id)">
                        Delete
                    </button>
                </td>
            </tr>
        }
    </tbody>
</table>

@code {

    List<User> users = new();

    string newUserName = "";

    int editId = 0;

    string editName = "";

    protected override async Task OnInitializedAsync()
    {
        await LoadUsers();
    }

    async Task LoadUsers()
    {
        users = await UserService.GetUsersAsync();
    }

    async Task AddUser()
    {
        await UserService.AddUserAsync(newUserName);

        newUserName = "";

        await LoadUsers();
    }

    void EditUser(User user)
    {
        editId = user.Id;
        editName = user.Name;
    }

    async Task SaveUser()
    {
        await UserService.UpdateUserAsync(editId, editName);

        editId = 0;

        await LoadUsers();
    }

    async Task DeleteUser(int id)
    {
        await UserService.DeleteUserAsync(id);

        await LoadUsers();
    }
}
```

---

# 6. SQL Server Table

```sql
CREATE TABLE Users
(
    Id INT PRIMARY KEY IDENTITY,
    Name NVARCHAR(100)
);
```

---

# 7. Navigation Menu

## `Layout/NavMenu.razor`

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="users">
        Users
    </NavLink>
</div>
```

---

# Important for Android/iOS

Direct SQL Server connection from mobile apps is NOT recommended because:

* Database credentials are exposed
* Firewall/network issues
* Security risks
* SQL Server not accessible publicly

Recommended architecture:

```text
MAUI Blazor App
       ↓
ASP.NET Core Web API
       ↓
SQL Server
```

---

# Recommended Production Stack

| Platform        | Database Access               |
| --------------- | ----------------------------- |
| Windows Desktop | Direct SQL Server OK          |
| Android/iOS     | Use Web API                   |
| Offline Mobile  | SQLite                        |
| Enterprise App  | ASP.NET Core API + SQL Server |

---

# Better Option for MAUI Mobile

Use:

* SQLite for offline storage
* ASP.NET Core Web API for server sync
* SQL Server as backend DB

---

# Recommended MAUI Architecture

```text
Maui Blazor Hybrid
│
├── Services
│   ├── ApiService.cs
│   ├── AuthService.cs
│
├── Models
│   └── User.cs
│
├── Pages
│   └── Users.razor
│
└── Web API
    ├── Controllers
    ├── Entity Framework
    └── SQL Server
```
