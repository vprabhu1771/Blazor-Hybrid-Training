This error usually means:

```text
Shared Memory Provider, error: 0 -
No process is on the other end of the pipe
```

The app reached SQL Server, but the SQL Server login/authentication failed or the SQL Server service closed the connection.

---

# Common Causes & Fixes

## 1. SQL Server Service Not Running

Open:

```text
SQL Server Configuration Manager
```

Check:

```text
SQL Server (MSSQLSERVER)
```

or

```text
SQL Server (SQLEXPRESS)
```

Status must be:

```text
Running
```

---

# 2. Wrong Server Name

Try these connection strings.

## LocalDB

```csharp
"Server=(localdb)\\MSSQLLocalDB;Database=YourDB;Trusted_Connection=True;TrustServerCertificate=True"
```

---

## SQL Express

```csharp
"Server=.\\SQLEXPRESS;Database=YourDB;Trusted_Connection=True;TrustServerCertificate=True"
```

---

## Default SQL Server

```csharp
"Server=localhost;Database=YourDB;Trusted_Connection=True;TrustServerCertificate=True"
```

---

# 3. SQL Authentication Disabled

If using:

```csharp
User ID=sa;Password=...
```

Then SQL Server must allow SQL logins.

Open:

## SQL Server Management Studio

### Steps

```text
Server Properties
    → Security
        → SQL Server and Windows Authentication mode
```

Enable:

```text
SQL Server and Windows Authentication mode
```

Restart SQL Server after changing.

---

# 4. `sa` Account Disabled

Run in SSMS:

```sql
ALTER LOGIN sa ENABLE;
GO

ALTER LOGIN sa WITH PASSWORD = 'YourStrongPassword123';
GO
```

---

# 5. Use Windows Authentication Instead

Instead of:

```csharp
User ID=sa;
Password=123;
```

Use:

```csharp
Trusted_Connection=True;
```

Example:

```csharp
string conn =
"Server=localhost;" +
"Database=YourDB;" +
"Trusted_Connection=True;" +
"TrustServerCertificate=True";
```

This is the easiest solution for local development.

---

# 6. Enable TCP/IP

Open:

```text
SQL Server Configuration Manager
```

Go to:

```text
SQL Server Network Configuration
    → Protocols for MSSQLSERVER
```

Enable:

* TCP/IP
* Named Pipes

Restart SQL Server.

---

# 7. Wrong Database Name

Verify database exists:

```sql
SELECT name FROM sys.databases
```

---

# 8. Test Connection in SSMS First

Before MAUI app:

Try login in:

* SQL Server Management Studio

If SSMS cannot connect, app also cannot connect.

---

# Recommended Working Local Connection String

## Windows Authentication

```csharp
string connString =
"Server=localhost;" +
"Database=YourDatabase;" +
"Trusted_Connection=True;" +
"TrustServerCertificate=True";
```

---

# Recommended for SQL Express

```csharp
string connString =
"Server=.\\SQLEXPRESS;" +
"Database=YourDatabase;" +
"Trusted_Connection=True;" +
"TrustServerCertificate=True";
```

---

# If Using LocalDB

```csharp
string connString =
"Server=(localdb)\\MSSQLLocalDB;" +
"Database=YourDatabase;" +
"Trusted_Connection=True;";
```

---

# Debug Test Code

Try this minimal code:

```csharp
using Microsoft.Data.SqlClient;

try
{
    string conn =
        "Server=localhost;" +
        "Database=master;" +
        "Trusted_Connection=True;" +
        "TrustServerCertificate=True";

    using SqlConnection sql = new(conn);

    await sql.OpenAsync();

    Console.WriteLine("Connected");
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

---

# For MAUI Android/iOS

Direct SQL Server connection usually fails because:

* SQL Server inaccessible externally
* Shared memory only works locally
* Mobile cannot use local SQL Server instance

Use:

```text
MAUI App
   ↓
ASP.NET Core Web API
   ↓
SQL Server
```

instead.
