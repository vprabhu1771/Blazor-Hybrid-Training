Create a new file:

`Shared/EmptyLayout.razor`
or
`Layouts/EmptyLayout.razor`

Add this code:

```razor id="27s0gj"
@inherits LayoutComponentBase

<div class="empty-layout">
    @Body
</div>
```

Then use it in your Login page:

```razor id="d7qz15"
@page "/login"
@layout EmptyLayout
@inject NavigationManager NavigationManager
```

If the layout is inside a different namespace/folder, add using:

```razor id="25enwa"
@using MauiApp.Layouts
```

Example:

```razor id="9kc4vl"
@page "/login"
@layout EmptyLayout
@using MauiApp.Layouts
```

Your final Login page header:

```razor id="c6b6ij"
@page "/login"
@layout EmptyLayout
@using MauiApp.Layouts
@inject NavigationManager NavigationManager
```
