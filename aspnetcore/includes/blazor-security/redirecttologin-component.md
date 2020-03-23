<span data-ttu-id="23635-101">El componente de `RedirectToLogin` (*Shared/RedirectToLogin. Razor*):</span><span class="sxs-lookup"><span data-stu-id="23635-101">The `RedirectToLogin` component (*Shared/RedirectToLogin.razor*):</span></span>

* <span data-ttu-id="23635-102">Administra la redirección de usuarios no autorizados a la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="23635-102">Manages redirecting unauthorized users to the login page.</span></span>
* <span data-ttu-id="23635-103">Conserva la dirección URL actual a la que el usuario intenta tener acceso para que se pueda devolver a esa página si la autenticación se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="23635-103">Preserves the current URL that the user is attempting to access so that they can be returned to that page if authentication is successful.</span></span>

```razor
@inject NavigationManager Navigation
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@code {
    protected override void OnInitialized()
    {
        Navigation.NavigateTo($"authentication/login?returnUrl={Navigation.Uri}");
    }
}
```