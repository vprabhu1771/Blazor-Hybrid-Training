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
