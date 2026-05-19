```
https://www.nuget.org/packages/QRCoder
```

# Create QR Code In Blazor Using ASP .Net Core
```
https://www.c-sharpcorner.com/article/create-qr-code-in-blazor-using-asp-net-core/
```

Install the package first:

## Install NuGet Package

Using terminal:

```bash
dotnet add package QRCoder
```

or in Visual Studio:

1. Right Click Project
2. Manage NuGet Packages
3. Browse
4. Search:

```text
QRCoder
```

5. Install official package from:

* [QRCoder NuGet](https://www.nuget.org/packages/QRCoder?utm_source=chatgpt.com)

---

Then use:

```csharp
@using QRCoder
```

## Working Imports

Your Razor page should start like this:

```razor
@page "/qr"

@using MauiApp1.Shared.Services
@using QRCoder

@inject IFormFactor FormFactor
```

---

## If Still Not Working

Clean and rebuild:

```bash
dotnet clean
dotnet build
```

or Visual Studio:

```text
Build → Clean Solution
Build → Rebuild Solution
```

---

## Check `.csproj`

Make sure package exists:

```xml
<ItemGroup>
    <PackageReference Include="QRCoder" Version="1.6.0" />
</ItemGroup>
```

---

## For .NET MAUI Blazor

Recommended versions:

* .NET 8
* QRCoder 1.6+

Works correctly on:

* Android
* iOS
* Windows
* MacCatalyst

## Working MAUI Blazor Android Code

```razor
@page "/qr"

@using MauiApp1.Shared.Services
@using QRCoder

@inject IFormFactor FormFactor

<PageTitle>QRComponent</PageTitle>

<h1>QRComponent</h1>

Welcome to your new app running on <em>@factor</em> using <em>@platform</em>.

<div class="input-group">
    <div class="col-sm-6">
        <label class="mb-3">QR Code Text</label>

        <input type="text"
               @bind="QRCodeText"
               placeholder="Enter your text"
               class="form-control mb-4" />

        <button @onclick="GenerateQRCode"
                class="btn btn-success">
            Generate QR Code
        </button>
    </div>
</div>

@if (!string.IsNullOrEmpty(QRByte))
{
    <img src="@QRByte" width="300" class="mb-5" />
}

@code {

    private string factor => FormFactor.GetFormFactor();
    private string platform => FormFactor.GetPlatform();

    public string QRCodeText { get; set; } = "";

    public string QRByte { get; set; } = "";

    public void GenerateQRCode()
    {
        if (!string.IsNullOrWhiteSpace(QRCodeText))
        {
            QRCodeGenerator qrGenerator = new();

            QRCodeData qrCodeData =
                qrGenerator.CreateQrCode(QRCodeText,
                QRCodeGenerator.ECCLevel.Q);

            PngByteQRCode qrCode = new(qrCodeData);

            byte[] qrCodeBytes = qrCode.GetGraphic(20);

            string base64 = Convert.ToBase64String(qrCodeBytes);

            QRByte = $"data:image/png;base64,{base64}";
        }
    }
}
```