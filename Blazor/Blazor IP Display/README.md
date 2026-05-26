```razor
@page "/showip"
@inject HttpClient Http

<h3>My Public IP Address</h3>

@if (string.IsNullOrEmpty(ipAddress))
{
    <p>Loading...</p>
}
else
{
    <p>Your IP: <strong>@ipAddress</strong></p>
}

@code {
    private string? ipAddress;

    protected override async Task OnInitializedAsync()
    {
        try
        {
            var result = await Http.GetFromJsonAsync<IpResponse>(
                "https://api.ipify.org/?format=json");

            ipAddress = result?.ip;
        }
        catch (Exception ex)
        {
            ipAddress = $"Error: {ex.Message}";
        }
    }

    public class IpResponse
    {
        public string? ip { get; set; }
    }
}
```

For .NET MAUI Blazor Hybrid, make sure `HttpClient` is registered in `MauiProgram.cs`:

```csharp
builder.Services.AddScoped(sp => new HttpClient());
```

Then visit:

```text
/showip
```
