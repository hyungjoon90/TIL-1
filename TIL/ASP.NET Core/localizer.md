# ASP.NET Core 다국어(세계화 및 지역화)

ASP.NET Core는 번역이 포함된 XML파일 형식인 리소스 파일들을 기반으로 합니다.
문화권을 전달하면 해당 문화권에 정의된 이름을 기반으로 리소스 파일을 찾아서 리소스 파일에 정의된 값이 표시됩니다.
아래 처럼 커스텀된 IHtmlLocalizer와 IStringLocalizer를 이용하면 하나의 리소스 파일에서 번역 리소스들을 관리할 수 있습니다.
그리고 해당 값이 번역이 되어있는 값인지도 확인이 가능합니다.

```csharp
namespace Sample.Language
{
    public class SampleHtmlLocalizer : IHtmlLocalizer
    {
        private readonly IHostingEnvironment _environment;

        public SampleHtmlLocalizer(IHostingEnvironment environment)
        {
            _environment = environment;
        }

        public LocalizedHtmlString this[string name]
        {
            get
            {
                var localizedString = GetString(name);
                return new LocalizedHtmlString(localizedString.Name, localizedString.Value, localizedString.ResourceNotFound);
            }
        }

        public LocalizedHtmlString this[string name, params object[] arguments]
        {
            get
            {
                var localizedString = GetString(name);
                return new LocalizedHtmlString(localizedString.Name, localizedString.Value, localizedString.ResourceNotFound, arguments);
            }
        }

        public IEnumerable<LocalizedString> GetAllStrings(bool includeParentCultures)
        {
            throw new NotImplementedException();
        }

        public LocalizedString GetString(string name)
        {
            var exsits = IsFoundResource(name, out string value);
            return new LocalizedString(name, value, exsits);
        }

        public LocalizedString GetString(string name, params object[] arguments)
        {
            var exsits = IsFoundResource(name, out string value);
            return new LocalizedString(name, string.Format(value, arguments), exsits);
        }

        public IHtmlLocalizer WithCulture(CultureInfo culture)
        {
            CultureInfo.DefaultThreadCurrentCulture = culture;
            return new SampleHtmlLocalizer(_environment);
        }

        private bool IsFoundResource(string name, out string value)
        {
            bool isReady = true;
            value = Resource.ResourceManager.GetString(name);

            if (string.IsNullOrEmpty(value))
            {
                value = name;
                isReady = false;
            }

            if (!_environment.IsProduction())
                value = $"{value}{(isReady ? "(O)" : "(X)")}";

            return isReady;
        }
    }
}
```

```csharp
namespace Sample.Language
{
    public class SampleHtmlLocalizerFactory : IHtmlLocalizerFactory
    {
        private readonly IHostingEnvironment _environment;

        public SampleHtmlLocalizerFactory(IHostingEnvironment environment)
        {
            _environment = environment;
        }

        public IHtmlLocalizer Create(Type resourceSource)
        {
            return new SampleHtmlLocalizer(_environment);
        }

        public IHtmlLocalizer Create(string baseName, string location)
        {
            return new SampleHtmlLocalizer(_environment);
        }
    }
}
```

```csharp
namespace Sample.Language
{
    public class SampleStringLocalizer : IStringLocalizer
    {
        private readonly IHostingEnvironment _environment;

        public SampleStringLocalizer(IHostingEnvironment environment)
        {
            _environment = environment;
        }

        public LocalizedString this[string name]
        {
            get
            {
                var exists = IsFoundResource(name, out string value);
                return new LocalizedString(name, value, exists);
            }
        }

        public LocalizedString this[string name, params object[] arguments]
        {
            get
            {
                var exists = IsFoundResource(name, out string value);
                return new LocalizedString(name, string.Format(value, arguments), exists);
            }
        }

        public IEnumerable<LocalizedString> GetAllStrings(bool includeParentCultures)
        {
            throw new NotImplementedException();
        }

        public IStringLocalizer WithCulture(CultureInfo culture)
        {
            CultureInfo.DefaultThreadCurrentCulture = culture;
            return new SampleStringLocalizer(_environment);
        }

        private bool IsFoundResource(string name, out string value)
        {
            bool isReady = true;
            value = Resource.ResourceManager.GetString(name);

            if (string.IsNullOrEmpty(value))
            {
                value = name;
                isReady = false;
            }

            if (!_environment.IsProduction())
                value = $"{value}{(isReady ? "(O)" : "(X)")}";

            return isReady;
        }
    }
}
```

```csharp
namespace Sample.Language
{
    public class SampleStringLocalizerFactory : IStringLocalizerFactory
    {
        private readonly IHostingEnvironment _environment;

        public SampleStringLocalizerFactory(IHostingEnvironment environment)
        {
            _environment = environment;
        }

        public IStringLocalizer Create(Type resourceSource)
        {
            return new SampleStringLocalizer(_environment);
        }

        public IStringLocalizer Create(string baseName, string location)
        {
            return new SampleStringLocalizer(_environment);
        }
    }
}
```


IHtmlLocalizerFactory 는 View단에서 지역화를 할 때 필요하다. Microsoft.AspNetCore.Mvc.Localization를 참조하고 있다.
IStringLocalizerFactory 는 View단 이외에서 지역화를 할 때 필요한데, Microsoft.Extensions.Localization를 참조하고 있다.

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .AddViewLocalization()
    .AddDataAnnotationsLocalization();
				
services.AddSingleton<IStringLocalizerFactory, SampleStringLocalizerFactory>();
services.AddTransient<IStringLocalizer, SampleStringLocalizer>();
services.AddSingleton<IHtmlLocalizerFactory, SampleHtmlLocalizerFactory>();
services.AddTransient<IHtmlLocalizer, SampleHtmlLocalizer>();
```

Startup.cs의 ConfigureServices에서 서비스를 위처럼 추가해준다.

```csharp
var supportedCultures = new[]
{
	new CultureInfo("en-US"),
    new CultureInfo("ko-KR"),
};

app.UseRequestLocalization(new RequestLocalizationOptions
{
    DefaultRequestCulture = new RequestCulture("en-US"),
    SupportedCultures = supportedCultures,
    SupportedUICultures = supportedCultures
});
```
Startup.cs의 Configure에서 요청 파이프 라인 구성은 위처럼 추가해준다.

Language 네임스페이스 프로젝트 파일 구성은 아래와 같다.

Sample.Language
├─ SampleHtmlLocalizer.cs
├─ SampleHtmlLocalizerFactory.cs
├─ SampleStringLocalizer.cs
├─ SampleStringLocalizerFactory.cs
└─ Resource.en.resx
└─ Resource.ko.resx
└─ Resource.resx

DataAnnotations 지역화를 사용하기 위해서는
Model이나 ViewModel에서 사용하는 방법이다.
```csharp
[Required(ErrorMessage = "{0}을(를) 입력해 주세요.")]
```
{0}을(를) 입력해 주세요. 가 리소스 파일에 번역되어 있다면 된다.

컨트롤러단에서는 아래와 같이 사용한다.

```csharp
private readonly IStringLocalizer _localizer;

public AccountController(IStringLocalizer localizer)
{
	_localizer = localizer;
}
	
ModelState.AddModelError(string.Empty, _localizer["사용자 ID 혹은 비밀번호가 올바르지 않습니다."]);
```

View단에서는 아래처럼 사용한다.
```csharp
@inject Microsoft.AspNetCore.Mvc.Localization.IHtmlLocalizer htmlLocalizer;
<p>@htmlLocalizer["안녕"]</p>
```

이제 언어를 변경하는 리스트를 만들때에는 아래와 같이 만들면 된다.
```
@using Microsoft.AspNetCore.Localization

@inject Microsoft.AspNetCore.Mvc.Localization.IHtmlLocalizer htmlLocalizer;

@model IList<QPlexBrand.Data.CommonDatabase.Models.AccountLoginLog>

@{
    ViewData["Title"] = "Home";

    var requestCulture = Context.Features.Get<IRequestCultureFeature>();
}

@using (Html.BeginForm("SetLanguage", "Home", new { area = "" }, FormMethod.Post, true, new { id = "language" }))
{
    <input type="hidden" name="returnUrl" value="/Home/Index" />
    <select name="culture" onchange="$('#language').submit();">
        @{
            switch (requestCulture.RequestCulture.Culture.ToString())
            {
                case "ko-KR":
                    <option value="ko-KR" selected>KR</option>
                    <option value="en-US">EN</option>
                    break;
                case "en-US":
                    <option value="ko-KR">KR</option>
                    <option value="en-US" selected>EN</option>
                    break;
                default:
                    <option value="ko-KR" selected>KR</option>
                    <option value="en-US">EN</option>
                    break;
            }
        }
    </select>
}
```

```
[HttpPost]
[AllowAnonymous]
public IActionResult SetLanguage(string culture, string returnUrl)
{
    Response.Cookies.Append(
        CookieRequestCultureProvider.DefaultCookieName,
        CookieRequestCultureProvider.MakeCookieValue(new RequestCulture(culture)),
        new CookieOptions { Expires = DateTimeOffset.UtcNow.AddYears(1) }
    );

    return LocalRedirect(returnUrl);
}
```

---
#### 참고

https://docs.microsoft.com/ko-kr/aspnet/core/fundamentals/localization?view=aspnetcore-2.2
