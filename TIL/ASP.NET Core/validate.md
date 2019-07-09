# ASP.NET Core 프론트엔드와 백엔드에서의 간단한 유효성 검사

클라이언트단에서의 유효성 검사의 장점으로는 서버단으로 넘어가기 전에 사용자의 브라우저 환경(클라이언트)에서 검사를 하기 때문에 서버 리소스의 낭비를 줄일 수 있다. 단점으로는 사용자가 유효성검사를 피하고 정보를 입력할 수 있다는 점이다. 그러면 서버단에서의 유효성 검사로는 이 클라이언트단에서의 유효성 검사를 피하고 들어오는 부분에 대해서 서버단에서는 조작이 힘들기 때문에 확실하게 유효성 검사가 가능하다.

구글링을 하다보니까 클라이언트단에서의 유효성 검사만 하는 개발자분들도 있고 서버단에서만 유효성 검사를 하는 개발자분들도 보았다. 개인적으로 두 부분에서 전부 유효성 검사를 하는게 맞는 듯 싶다. 불필요한 서버 리소스를 막을 수 있고, 클라이언트단 유효성 검사를 피하고 들어오더라도 서버단에서 2중으로 유효성 검사를 하기 때문이다.

.NET Core에서 유효성 검사는 더 간단해졌다.
Views - Shared에 _ValidationScriptsPartial가 있어서 필요한 View페이지에서는 
```csharp
@{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
```
사용할 수 있다.

View페이지의 @model에 사용되는 Viewmodel 또는 model에 유효성 검사 규칙을 지정할 수 있다.

```csharp
public class RegisterViewModel
{
    [Required(ErrorMessage = "{0}을(를) 입력해 주세요.")]
    [RegularExpression(@"^[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.)|(([\w-]+\.)+))([a-zA-Z]{2,4}|[0-9]{1,3})(\]?)$", ErrorMessage = "이메일 형식이 올바르지 않습니다.")]
    [Display(Name = "이메일")]
    public string Email { get; set; }

    [Required(ErrorMessage = "{0}을(를) 입력해 주세요.")]
    [Display(Name = "이름")]
    public string CustomerName { get; set; }

    [Required(ErrorMessage = "{0}을(를) 입력해 주세요.")]
    [StringLength(16, ErrorMessage = "{0}은(는) 최소 {2}, 최대 {1} 글자여야 합니다.", MinimumLength = 8)]
    [DataType(DataType.Password)]
    [RegularExpression(@"^(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,}$", ErrorMessage = "비밀번호 형식이 올바르지 않습니다.")]
    [Display(Name = "비밀번호")]
    public string Password { get; set; }

    [Required(ErrorMessage = "{0}을(를) 입력해 주세요.")]
    [DataType(DataType.Password)]
    [Display(Name = "비밀번호 확인")]
    [Compare("Password", ErrorMessage = "입력하신 비밀번호와 일치하지 않습니다.")]
    public string ConfirmPassword { get; set; }
}
```

프론트엔드에서는 폼 요소를 가져와서 .Vaild()를 사용하면 유효성 검사가 프론트엔드단에서 실행된다.
백엔드단으로 넘기기 전에 유효성 검사를 거칠 수 있고, 백엔드단으로 넘어가면서는

```csharp
if (!ModelState.IsValid)
{
    return View(viewModel);
}
```
로 유효성 검사를 할 수 있다.

아래는 View페이지 소스 전체이다.

```csharp
@model QPlexBrand.Data.ViewModels.RegisterViewModel

@{
    ViewData["Title"] = "Register";
}

<div class="container-scroller">
    <div class="container-fluid page-body-wrapper full-page-wrapper auth-page">
        <div class="content-wrapper d-flex align-items-center auth register-bg-1 theme-one">
            <div class="row w-100">
                <div class="col-lg-4 mx-auto">
                    <h2 class="text-center mb-4">회원가입</h2>
                    <div class="auto-form-wrapper">
                        <form method="post" id="registerAccount" asp-controller="Account" asp-action="Register">
                            <div class="form-group">
                                <div class="input-group">
                                    <input type="text" class="form-control" asp-for="Email" placeholder="아이디" onfocusin="onFocusEmail()" onfocusout="onFocusOutEmail()">
                                    <div class="input-group-append">
                                        <span class="input-group-text">
                                            <i class="mdi mdi-check-circle-outline"></i>
                                        </span>
                                    </div>
                                </div>
                                <span class="text-small text-danger font-weight-semibold" id="dataValEmail" asp-validation-for="Email"></span>
                            </div>
                            <div class="form-group">
                                <div class="input-group">
                                    <input type="text" class="form-control" asp-for="CustomerName" placeholder="이름">
                                    <div class="input-group-append">
                                        <span class="input-group-text">
                                            <i class="mdi mdi-check-circle-outline"></i>
                                        </span>
                                    </div>
                                </div>
                                <span class="text-small text-danger font-weight-semibold" asp-validation-for="CustomerName"></span>
                            </div>
                            <div class="form-group">
                                <div class="input-group">
                                    <input type="password" class="form-control" asp-for="Password" placeholder="비밀번호" onfocusout="onFocusOutPassword()">
                                    <div class="input-group-append">
                                        <span class="input-group-text">
                                            <i class="mdi mdi-check-circle-outline"></i>
                                        </span>
                                    </div>
                                </div>
                                <span class="text-small text-danger font-weight-semibold" asp-validation-for="Password"></span>
                            </div>
                            <div class="form-group">
                                <div class="input-group">
                                    <input type="password" class="form-control" asp-for="ConfirmPassword" placeholder="비밀번호 확인" onfocusout="onFocusOutConfirmPassword()">
                                    <div class="input-group-append">
                                        <span class="input-group-text">
                                            <i class="mdi mdi-check-circle-outline"></i>
                                        </span>
                                    </div>
                                </div>
                                <span class="text-small text-danger font-weight-semibold" asp-validation-for="ConfirmPassword"></span>
                            </div>
                            <div class="text-small text-danger font-weight-semibold" asp-validation-summary="ModelOnly"></div>
                            <div class="form-group">
                                <button class="btn btn-primary submit-btn btn-block" id="btnRegister">회원가입</button>
                            </div>
                            <div class="text-block text-center my-3">
                                <span class="text-small font-weight-semibold">이미 회원이신가요 ?</span>
                                <a class="text-black text-small" asp-controller="Account" asp-action="Login">로그인</a>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <!-- content-wrapper ends -->
    </div>
    <!-- page-body-wrapper ends -->
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
    <script>
        var beforeEmail, afterEmail = '';
        var $email = $('#Email');
        var $dataValEmail = $('#dataValEmail');
        var $password = $('#Password');
        var $confirmPassword = $('#ConfirmPassword');

        function onFocusEmail() {
            beforeEmail = $email.val();
        }

        function onFocusOutEmail() {
            afterEmail = $email.val();
            if (afterEmail !== '') {
                if ($email.valid()) {
                    if (beforeEmail !== afterEmail) {
                        emailDuplicationCheck();
                    }
                }
            }
        }

        function emailDuplicationCheck() {
            var data = {};
            var _token = $('input[name="__RequestVerificationToken"]').val();
            data.email = afterEmail;
            data.__RequestVerificationToken = _token;
            $.post('/Account/CheckEmail', data, function (res) {
                if (res.result) {
                    $dataValEmail.text(res.resultMsg);
                } else {
                    $dataValEmail.text(res.resultMsg);
                }
            })
        }

        function onFocusOutPassword() {
            if ($password.val() !== '') {
                $password.valid();
            }
        }

        function onFocusOutConfirmPassword() {
            if ($confirmPassword.val() !== '') {
                $confirmPassword.valid();
            }
        }
    </script>
}
```
