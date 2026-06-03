In **.NET MAUI**, you can access the device's GPS location using the **Geolocation API** from the `Microsoft.Maui.Essentials` namespace.

## 1. Get Current Location

```csharp
using Microsoft.Maui.Devices.Sensors;

public async Task<Location?> GetCurrentLocationAsync()
{
    try
    {
        GeolocationRequest request = new GeolocationRequest(
            GeolocationAccuracy.High,
            TimeSpan.FromSeconds(10));

        Location? location = await Geolocation.Default.GetLocationAsync(request);

        if (location != null)
        {
            Console.WriteLine($"Latitude: {location.Latitude}");
            Console.WriteLine($"Longitude: {location.Longitude}");
        }

        return location;
    }
    catch (FeatureNotSupportedException)
    {
        await Shell.Current.DisplayAlert("Error", "GPS not supported.", "OK");
    }
    catch (FeatureNotEnabledException)
    {
        await Shell.Current.DisplayAlert("Error", "GPS is disabled.", "OK");
    }
    catch (PermissionException)
    {
        await Shell.Current.DisplayAlert("Error", "Location permission denied.", "OK");
    }

    return null;
}
```

---

## 2. Request Location Permission

### Android

Add permissions in **Platforms/Android/AndroidManifest.xml**

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

For Android 10+ background access:

```xml
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

---

### iOS

Add to **Platforms/iOS/Info.plist**

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app requires location access.</string>

<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>This app requires location access.</string>
```

---

## 3. Check Permission Before Accessing GPS

```csharp
using Microsoft.Maui.ApplicationModel;

public async Task<bool> RequestLocationPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();

    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
    }

    return status == PermissionStatus.Granted;
}
```

Usage:

```csharp
if (await RequestLocationPermission())
{
    var location = await GetCurrentLocationAsync();
}
```

---

## 4. Get Last Known Location

Faster than requesting GPS every time:

```csharp
Location? location = await Geolocation.Default.GetLastKnownLocationAsync();

if (location != null)
{
    Console.WriteLine(location.Latitude);
    Console.WriteLine(location.Longitude);
}
```

---

## 5. Reverse Geocoding (Latitude → Address)

```csharp
using Microsoft.Maui.Devices.Sensors;

var placemarks = await Geocoding.Default.GetPlacemarksAsync(
    location.Latitude,
    location.Longitude);

var place = placemarks?.FirstOrDefault();

string address =
    $"{place.Thoroughfare}, " +
    $"{place.Locality}, " +
    $"{place.AdminArea}, " +
    $"{place.CountryName}";
```

---

## Example in a MAUI Blazor Page

```razor
<button @onclick="GetLocation">Get Location</button>

<p>Latitude: @Latitude</p>
<p>Longitude: @Longitude</p>

@code {
    private double Latitude;
    private double Longitude;

    private async Task GetLocation()
    {
        var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();

        if (status == PermissionStatus.Granted)
        {
            var location = await Geolocation.Default.GetLocationAsync();

            if (location != null)
            {
                Latitude = location.Latitude;
                Longitude = location.Longitude;
            }
        }
    }
}
```

This works in **.NET MAUI Android**, **iOS**, **Windows**, and **MAUI Blazor Hybrid** applications.
