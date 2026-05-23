# Blazor Two-Way Binding Example

Two-way binding means:

* UI updates variable
* Variable updates UI

Blazor uses:

```razor
@bind
```

or

```razor
@bind-Value
```

---

# Simple Example

```razor id="4yl8ei"
@page "/binding"

<h3>Two-Way Binding</h3>

<input type="text"
       class="form-control"
       @bind="name" />

<h4>Hello @name</h4>

@code {
    private string name = "Prabhu";
}
```

### How it works

* User types in textbox
* `name` variable updates automatically
* UI refreshes automatically

---

# Using Blazor InputText

```razor id="k2wd3n"
@page "/binding2"

<h3>InputText Binding</h3>

<InputText class="form-control"
           @bind-Value="username" />

<p>User Name: @username</p>

@code {
    private string username = "";
}
```

---

# Number Binding

```razor id="6yqq7z"
<InputNumber class="form-control"
             @bind-Value="age" />

<p>Age: @age</p>

@code {
    private int age = 25;
}
```

---

# Checkbox Binding

```razor id="jlwmq9"
<InputCheckbox @bind-Value="isActive" />

<p>Status: @isActive</p>

@code {
    private bool isActive = true;
}
```

---

# Dropdown Binding

```razor id="y31m5u"
<InputSelect class="form-select"
             @bind-Value="country">

    <option value="">Select</option>
    <option>India</option>
    <option>USA</option>
    <option>UK</option>

</InputSelect>

<p>Country: @country</p>

@code {
    private string country = "";
}
```

---

# Real-Time Binding

Default binding happens on `change`.

For real-time typing updates:

```razor id="4egm61"
<input class="form-control"
       @bind="name"
       @bind:event="oninput" />

<p>@name</p>
```

* Updates while typing
* Not after losing focus

---

# One-Way vs Two-Way

| Type    | Syntax          | Description   |
| ------- | --------------- | ------------- |
| One-Way | `value="@name"` | UI only       |
| Two-Way | `@bind="name"`  | UI + Variable |

---

# Multiple Bindings Example

```razor id="r0h2eh"
@page "/profile"

<h3>Profile</h3>

<input class="form-control mb-2"
       placeholder="Name"
       @bind="name" />

<input class="form-control mb-2"
       placeholder="Email"
       @bind="email" />

<InputNumber class="form-control mb-2"
             @bind-Value="age" />

<hr />

<p>Name: @name</p>
<p>Email: @email</p>
<p>Age: @age</p>

@code {
    string name = "";
    string email = "";
    int age = 18;
}
```
