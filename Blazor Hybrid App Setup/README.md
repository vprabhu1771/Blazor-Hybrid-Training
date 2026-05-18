To build a **Blazor Hybrid** app, you need the **.NET MAUI workload** because Blazor Hybrid runs inside a native MAUI application using a `BlazorWebView`.

---

# 1. Install Required Software

## Install Visual Studio

Download:

[Visual Studio 2022](https://visualstudio.microsoft.com/vs/?utm_source=chatgpt.com)

During installation, select:

* **.NET Multi-platform App UI development**
* Optional:

  * Android SDK
  * Windows App SDK
  * iOS/macOS support (Mac required for iOS build)

---

# 2. Install .NET SDK

Download latest .NET SDK:

[.NET SDK Download](https://dotnet.microsoft.com/download?utm_source=chatgpt.com)

Verify:

```bash
dotnet --version
```

---

# 3. Install MAUI Workload

Open terminal / PowerShell:

```bash
dotnet workload install maui
```

Verify installed workloads:

```bash
dotnet workload list
```

You should see something like:

```bash
maui
maui-android
maui-windows
```

---

# 4. Create Blazor Hybrid Project

## Using CLI

```bash
dotnet new maui-blazor -n MyHybridApp
```

Open project:

```bash
cd MyHybridApp
```

Run app:

```bash
dotnet build
```

---

# 5. Open in Visual Studio

Open:

```text
MyHybridApp.sln
```

Run on:

* Windows Machine
* Android Emulator
* Physical Android Device

---

# 6. Project Structure

Typical Blazor Hybrid structure:

```text
MyHybridApp/
│
├── Platforms/
│   ├── Android/
│   ├── Windows/
│   └── iOS/
│
├── wwwroot/
│
├── Components/
│   ├── Pages/
│   └── Layout/
│
├── MainPage.xaml
├── MauiProgram.cs
└── App.xaml
```

---

# 7. How Blazor Hybrid Works

Architecture:

```text
Native MAUI App
      ↓
BlazorWebView
      ↓
Razor Components (.razor)
```

It uses:

* Native device APIs
* Web UI with Razor
* No browser needed

---

# 8. Important NuGet Packages

Usually preinstalled:

* `Microsoft.AspNetCore.Components.WebView.Maui`
* `Microsoft.Maui.Controls`

---

# 9. Run Android Emulator

In Visual Studio:

```text
Tools → Android → Android Device Manager
```

Create emulator and run.

---

# 10. Useful Commands

## Restore workloads

```bash
dotnet workload restore
```

## Update workloads

```bash
dotnet workload update
```

## Clean project

```bash
dotnet clean
```

---

# 11. Create Blazor Hybrid with Individual Platforms

## Android only

```bash
dotnet new maui-blazor -n MyApp -f net9.0-android
```

## Windows only

```bash
dotnet new maui-blazor -n MyApp -f net9.0-windows10.0.19041.0
```

---

# 12. Common Errors

## Workload not installed

```text
NETSDK1147: workloads must be installed
```

Fix:

```bash
dotnet workload install maui
```

---

## Android SDK missing

Open Visual Studio Installer → Modify → install Android SDK.

---

## Emulator not starting

Enable:

* BIOS virtualization
* Windows Hyper-V / WHPX

---

# 13. Official Documentation

* [.NET MAUI Documentation](https://learn.microsoft.com/dotnet/maui/?utm_source=chatgpt.com)
* [Blazor Hybrid Documentation](https://learn.microsoft.com/aspnet/core/blazor/hybrid/?utm_source=chatgpt.com)
* [MAUI CLI Templates](https://learn.microsoft.com/dotnet/maui/get-started/first-app?utm_source=chatgpt.com)
