Here’s a simple Blazor form example using `EditForm` with validation.

## Model

Create `Models/UserModel.cs`

```csharp
using System.ComponentModel.DataAnnotations;

namespace MauiApp.Models
{
    public class UserModel
    {
        [Required]
        public string Name { get; set; } = string.Empty;

        [Required]
        [EmailAddress]
        public string Email { get; set; } = string.Empty;

        [Range(18, 60)]
        public int Age { get; set; }
    }
}
```

---

## Blazor Page

Create `Pages/UserForm.razor`

```razor
@page "/userform"
@using MauiApp.Models

<h3>User Registration Form</h3>

<EditForm Model="@user" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="mb-3">
        <label>Name</label>
        <InputText class="form-control" @bind-Value="user.Name" />
    </div>

    <div class="mb-3">
        <label>Email</label>
        <InputText class="form-control" @bind-Value="user.Email" />
    </div>

    <div class="mb-3">
        <label>Age</label>
        <InputNumber class="form-control" @bind-Value="user.Age" />
    </div>

    <button type="submit" class="btn btn-primary">
        Submit
    </button>
</EditForm>

@if (!string.IsNullOrEmpty(message))
{
    <div class="alert alert-success mt-3">
        @message
    </div>
}

@code {
    private UserModel user = new();
    private string message = string.Empty;

    private void HandleSubmit()
    {
        message = $"Welcome {user.Name} ({user.Email})";
    }
}
```

---

## Add Menu Link

Inside `Layout/NavMenu.razor`

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="userform">
        User Form
    </NavLink>
</div>
```

---

## Output Features

This form includes:

* `EditForm`
* `InputText`
* `InputNumber`
* Validation
* Submit handling
* Success message

---

## Validation Example

If fields are empty:

* Name required
* Valid email required
* Age must be between 18–60

Blazor automatically shows validation messages using:

```razor
<DataAnnotationsValidator />
<ValidationSummary />
```

You can also show field-level validation:

```razor
<ValidationMessage For="@(() => user.Email)" />
```

Example:

```razor
<InputText class="form-control" @bind-Value="user.Email" />
<ValidationMessage For="@(() => user.Email)" />
```
