We can solve this issue by enabling the anonymity using the following code snippet that includes the `AllowAnonymous` attribute to the `Login` action method:

```
```csharp
[AllowAnonymous]
public IActionResult Login(string redirectUrl)
{
    ViewBag.RedirectUrl = redirectUrl;
    return View();
}
```
```

We will also need the `Login HttpPost` action to be anonymous since the user will not be authenticated while sending his login credentials to the server, so add `AllowAnonymous` to the following action as well:

```
```csharp
[AllowAnonymous]
[HttpPost]
public async Task<IActionResult> Login(LoginViewModel
loginViewModel)
{
    // Code removed for brevity
    var claims = new List<Claim>
    {
        new Claim(ClaimTypes.Name, loginViewModel.Username)
    };

    var claimsIdentity = new ClaimsIdentity(
        claims,
        CookieAuthenticationDefaults.AuthenticationScheme);

    await HttpContext.SignInAsync(
        CookieAuthenticationDefaults.AuthenticationScheme,
        new ClaimsPrincipal(claimsIdentity));
    return RedirectToAction("Index", "Home");
}
```
```

The authentication-related implementation is committed to the Git repository and is available in the following URL:

`goo.gl/XudiqY`