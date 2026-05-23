# Blazor Input Components Example

Blazor provides built-in input components for forms and data binding.

## Full Example

```razor
@page "/inputs"

<h3>Blazor Input Examples</h3>

<div class="mb-3">
    <label>Name</label>
    <InputText class="form-control" @bind-Value="name" />
</div>

<div class="mb-3">
    <label>Age</label>
    <InputNumber class="form-control" @bind-Value="age" />
</div>

<div class="mb-3">
    <label>Date of Birth</label>
    <InputDate class="form-control" @bind-Value="dob" />
</div>

<div class="mb-3">
    <label>Password</label>
    <InputText type="password"
               class="form-control"
               @bind-Value="password" />
</div>

<div class="mb-3">
    <label>Description</label>
    <InputTextArea class="form-control"
                   @bind-Value="description" />
</div>

<div class="form-check mb-3">
    <InputCheckbox class="form-check-input"
                   @bind-Value="isActive" />
    <label class="form-check-label">
        Active User
    </label>
</div>

<hr />

<h4>Output</h4>

<p><b>Name:</b> @name</p>
<p><b>Age:</b> @age</p>
<p><b>DOB:</b> @dob.ToShortDateString()</p>
<p><b>Password:</b> @password</p>
<p><b>Description:</b> @description</p>
<p><b>Active:</b> @isActive</p>

@code {
    private string name = "";
    private int age = 18;
    private DateTime dob = DateTime.Now;
    private string password = "";
    private string description = "";
    private bool isActive = false;
}
```

---

# Common Blazor Input Components

| Component       | Purpose         |
| --------------- | --------------- |
| `InputText`     | Text input      |
| `InputNumber`   | Numeric input   |
| `InputDate`     | Date picker     |
| `InputCheckbox` | Checkbox        |
| `InputSelect`   | Dropdown        |
| `InputTextArea` | Multi-line text |

---

# Dropdown Example

```razor
<InputSelect class="form-select" @bind-Value="country">
    <option value="">Select Country</option>
    <option>India</option>
    <option>USA</option>
    <option>UK</option>
</InputSelect>

<p>Selected: @country</p>

@code {
    private string country = "";
}
```

---

# Two-Way Binding

Blazor uses:

```razor
@bind-Value
```

Example:

```razor
<InputText @bind-Value="name" />
```

Equivalent to:

```razor
value="@name"
@onchange="..."
```

---

# HTML Input Alternative

You can also use normal HTML inputs:

```razor
<input type="text" @bind="name" class="form-control" />
```

But Blazor components (`InputText`, `InputNumber`) work better with validation and `EditForm`.
