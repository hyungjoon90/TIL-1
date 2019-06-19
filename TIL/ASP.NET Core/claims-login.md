# ASP.NET Core 클레임 기반 로그인 구현

Claims을 이용한 구현 방법이다. Role이나 policy를 이용해서 세부적인 권한까지도 다룰 수 있으며, 특정 시간동안 사용자가 반응이 없다면 로그아웃까지 간단하게 구현할 수 있다.

```csharp
#region Authentication
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme).AddCookie();
services.ConfigureApplicationCookie(options =>
{
    options.ExpireTimeSpan = TimeSpan.FromHours(1);
    options.LoginPath = "/Account/LogIn";
    options.LogoutPath = "/Account/LogOut";
    options.AccessDeniedPath = "/Home/Index";
    options.SlidingExpiration = true;
});
#endregion
```

컨테이너에 해당 서비스를 추가한다.

```csharp
app.UseAuthentication();
```

요청 파이프라인을 구성한다.

```csharp
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
public async Task<IActionResult> LogIn(LoginViewModel model)
{
    if (!ModelState.IsValid)
    {
        return View(model);
    }

    var account = await _accountService.LogInAsync(model.Email, model.Password);

    if (account != null)
    {
        var claims = BuildClaims(account);

        var claimsidentity = new ClaimsIdentity(claims, CookieAuthenticationDefaults.AuthenticationScheme);
        await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, new ClaimsPrincipal(claimsidentity));
        return RedirectToAction("Index", "Home");
    }

    ModelState.AddModelError(string.Empty, _localizer["사용자 이메일 혹은 비밀번호가 올바르지 않습니다."]);
    return View(model);
}
```

```csharp
private IList<Claim> BuildClaims(CommonAccount commonAccount)
{
    var roles = "User";
    if (commonAccount.IsSuperAdmin)
    {
        roles = "Admin";
    }
    else
    {
        roles = commonAccount.FirstSubscriber ? "Manager" : "User";
    }

    var claims = new List<Claim>
    {
        new Claim(ClaimTypes.Sid, $"{commonAccount.Id}"),
        new Claim("Email", commonAccount.Email),
        new Claim("Name", commonAccount.AccountName),
        new Claim(ClaimTypes.Role, roles),

    };

    return claims;
}
```