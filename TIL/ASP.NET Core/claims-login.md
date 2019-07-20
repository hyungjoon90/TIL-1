# ASP.NET Core 클레임 기반 로그인 구현

Claims을 이용한 구현 방법이다. Role이나 policy를 이용해서 세부적인 권한까지도 다룰 수 있으며, 특정 시간동안 사용자가 반응이 없다면 로그아웃까지 간단하게 구현할 수 있다.

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme).AddCookie();
services.ConfigureApplicationCookie(options =>
{
    options.ExpireTimeSpan = TimeSpan.FromHours(1);
    options.LoginPath = "/Account/LogIn";
    options.LogoutPath = "/Account/LogOut";
    options.AccessDeniedPath = "/Home/Index";
});
```
```options.ExpireTimeSpan```는 쿠키가 만료될 시간 간격을 지정하는 값이다. 이 만료시간을 쿠키가 생성된 이후가 아닌 사용자의 조작이 없을 때를 기준으로 한다.
사용자의 조작이 10:30에 있다면 쿠키의 만료시간은 11:30으로 갱신되게 된다. 그리고 사용자의 조작이 없이 1시간이 지나면 만료시간이 지나기 때문에 삭제된다. 결국 쿠키가 없기 때문에 ```options.LogoutPath```에 설정된 경로로 로그아웃을 하게 된다.

```options.LoginPath```를 설정하면 만약 로그인이 필요한 페이지의 경우 자동으로 리디렉션 된다. ```options.AccessDeniedPath```는 엑세스가 거부되었을 때의 경로이다.

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

위에서 구현한 로그인 코드들은 로그인한 사용자만 접속 가능한 페이지에 들어가면 로그인 페이지로 리디렉션 하도록 되어있다.

그러고 만약 로그인을 다시 해서 성공하면 위의 코드는 Home의 Index로 가도록 되어있지 접속 하려다가 막힌 페이지로 다시 리디렉션 하지 않게 되어있다.
사용자가 특정 페이지로 리디렉션 하도록 하려면 returnUrl을 이용해야 한다.

로그인 페이지를 보여주는 컨트롤러를 아래처럼 구성한다.

```csharp
[AllowAnonymous]
public IActionResult LogIn(string returnUrl)
{
    if (User.Identity.IsAuthenticated)
    {
        return RedirectToAction("Index", "Home");
    }

    ViewBag.ReturnUrl = returnUrl;
    return View();
}
```

returnUrl을 가지고 있도록 하면 로그인이 필요해서 넘어오기 전의 사용자가 보려고 했던 페이지 URL을 returnUrl변수가 가지고 있게 된다.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction("Index", "Home");
    }
}
```

그리고 이전의 Url로 리다이렉트해주는 함수를 하나 만든다. 위의 함수는 로그인을 하려는 Submit 또는 Ajax Controller에서 returnUrl을 변수로 받아서 로그인을 성공하면 RedirectToLocal를 호출하게 되고 returnUrl이 있다면 returnUrl로 리다이렉트 해줄것이다.

```csharp
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
public async Task<IActionResult> LogIn(LoginViewModel model, string returnUrl)
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

        return RedirectToLocal(returnUrl);
    }

    ModelState.AddModelError(string.Empty, _localizer["사용자 이메일 혹은 비밀번호가 올바르지 않습니다."]);
    return View(model);
}
```

사용자가 A라는 페이지(로그인을 해야만 접근 가능한 페이지라고 가정)에 접속했을 때, 권한이 없기 때문에 로그인 페이지로 돌아갔을 때, 사용자는 로그인하고 다시 A페이지를 찾아서 들어가는 수고를 들일 필요 없이 로그인을 성공하면 A라는 페이지로 리다이렉트 된다.

### .NET과 .NET Core의 Authentication 차이

```csharp
IAuthenticationManager Authentication
{
    get { return HttpContext.GetOwinContext().Authentication; }
}

Authentication.SignIn(new AuthenticationProperties(), identity);
```

.NET에서 Authentication은 GetOwinContext라는 확장메서드를 이용해서 Authentication를 사용할 수 있다.

```csharp
await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, new ClaimsPrincipal(claimsIdentity));
```

.NET Core에서는 MVC를 시작할 때 설치되는 Microsoft.AspNetCore.App(2.2.0)에 Authentication과 관련된 모든 것들이 설치되어 있다.
StartUp에서 컨테이너에 Authentication서비스를 추가하면 HttpContext에서 사용할 수 있다.

