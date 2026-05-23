# Blazor Form Validation Example

Blazor form validation is commonly done using:

* `EditForm`
* `DataAnnotationsValidator`
* Data Annotation attributes

---

# Step 1 — Create Model

Create `Models/RegisterModel.cs`

```csharp id="ahjls1"
using System.ComponentModel.DataAnnotations;

namespace MauiApp.Models
{
    public class RegisterModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; } = "";

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email")]
        public string Email { get; set; } = "";

        [Required]
        [StringLength(10, MinimumLength = 4)]
        public string Password { get; set; } = "";

        [Range(18, 60, ErrorMessage = "Age must be between 18 and 60")]
        public int Age { get; set; }
    }
}
```

---

# Step 2 — Create Form Page

Create `Pages/Register.razor`

```razor id="r1a0dl"
@page "/register"
@using MauiApp.Models

<h3>Register Form</h3>

<EditForm Model="@model"
          OnValidSubmit="HandleValidSubmit">

    <DataAnnotationsValidator />

    <ValidationSummary />

    <div class="mb-3">
        <label>Name</label>

        <InputText class="form-control"
                   @bind-Value="model.Name" />

        <ValidationMessage For="@(() => model.Name)" />
    </div>

    <div class="mb-3">
        <label>Email</label>

        <InputText class="form-control"
                   @bind-Value="model.Email" />

        <ValidationMessage For="@(() => model.Email)" />
    </div>

    <div class="mb-3">
        <label>Password</label>

        <InputText type="password"
                   class="form-control"
                   @bind-Value="model.Password" />

        <ValidationMessage For="@(() => model.Password)" />
    </div>

    <div class="mb-3">
        <label>Age</label>

        <InputNumber class="form-control"
                     @bind-Value="model.Age" />

        <ValidationMessage For="@(() => model.Age)" />
    </div>

    <button class="btn btn-primary" type="submit">
        Register
    </button>

</EditForm>

@if (!string.IsNullOrEmpty(message))
{
    <div class="alert alert-success mt-3">
        @message
    </div>
}

@code {
    private RegisterModel model = new();

    private string message = "";

    private void HandleValidSubmit()
    {
        message = $"Welcome {model.Name}";
    }
}
```

---

# Validation Components

| Component                  | Purpose                  |
| -------------------------- | ------------------------ |
| `EditForm`                 | Form container           |
| `DataAnnotationsValidator` | Enables model validation |
| `ValidationSummary`        | Shows all errors         |
| `ValidationMessage`        | Shows field error        |

---

# Validation Attributes

| Attribute        | Example           |
| ---------------- | ----------------- |
| `[Required]`     | Required field    |
| `[EmailAddress]` | Email validation  |
| `[Range]`        | Number range      |
| `[StringLength]` | Min/max length    |
| `[Compare]`      | Compare passwords |

---

# Password Confirm Example

```csharp id="v8pwk6"
[Compare("Password", ErrorMessage = "Passwords do not match")]
public string ConfirmPassword { get; set; } = "";
```

---

# Form Events

| Event             | Description          |
| ----------------- | -------------------- |
| `OnValidSubmit`   | Runs when form valid |
| `OnInvalidSubmit` | Runs when invalid    |
| `OnSubmit`        | Always runs          |

Example:

```razor id="l6eqbk"
<EditForm Model="@model"
          OnInvalidSubmit="HandleInvalid">
</EditForm>
```

---

# Real-Time Validation

Validate while typing:

```razor id="u7vbh6"
<InputText class="form-control"
           @bind-Value="model.Name"
           @bind-Value:event="oninput" />
```

---

# Add Navigation Menu

`Layout/NavMenu.razor`

```razor id="trz8lt"
<NavLink class="nav-link" href="register">
    Register
</NavLink>
```
