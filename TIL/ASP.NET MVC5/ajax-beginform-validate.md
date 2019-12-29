# Ajax.BeginForm에서 Json으로 유효성 검사

Ajax.BeginForm로 Json을 반환하는 컨트롤러와 뷰는 Html.BeginForm에서 처럼 ModelState.AddModelErrorr가 작동하지 않는다.

return Json(new { status = false, error = ModelState.Values.Where(i => i.Errors.Count > 0).Select(i => i.Errors.Select(m => m.ErrorMessage)) });

위처럼 반환해서 메시지로 뿌리는 방법이 있고, 프론트엔드에서 유효성검사 한 것처럼 Validation 위치에 텍스트를 넣어주는 방법은 아래처럼 하면 된다.

개별 속성에 대한 ModelState 오류를 나타내는 클래스를 만든다.

```csharp
public class ValidationError
{

    public string PropertyName = "";
    public string[] ErrorList = null;
}
```

ModelState를 기반으로 ValidationErrors 목록을 리턴하는 메서드 작성

```csharp
public IEnumerable<ValidationError> GetModelStateErrors(ModelStateDictionary modelState)
{
    var errors = (from m in modelState
                        where m.Value.Errors.Count() > 0
                        select
                           new ValidationError
                           {
                               PropertyName = m.Key,
                               ErrorList = (from msg in m.Value.Errors
                                              select msg.ErrorMessage).ToArray()
                           })
                        .AsEnumerable();
    return errors;
}
```

그런 다음 컨트롤러의 Post 메서드에서 다음을 수행한다.

```csharp
if (!ModelState.IsValid)
{
    return Json(new
    {
        errors = true,
        errorList = GetModelStateErrors(ModelState)
    }, JsonRequestBehavior.AllowGet);
}
```

위에서 반환 된 오류 목록을 반복하는 JS 함수를 만들 수 있다.

```javascript
$.ajax({
            cache: false,
            async: true,
            type: "POST",
            url: form.attr('action'),
            data: form.serialize(),
            success: function (data) {
                if (data.errors) {
                    displayValidationErrors(data.errorList);
                 }
            },
        error: function (result) {
            console.log("Error");
        }

    });

function displayValidationErrors(errors) {
    $.each(errors, function (idx, validationError) {

        $("span[data-valmsg-for='" + validationError.PropertyName + "']").text(validationError. ErrorList[0]);

    });
}
```

위의 예에서는 'ErrorList'에서 첫 번째 오류 메시지만 받고 있다. 모든 메시지를 가져오고 유효성 검사 범위에 추가하기 위해 추가 루프를 만들 수 있다.

---
#### 참고

https://stackoverflow.com/questions/7287412/jquery-validate-asp-net-mvc-modelstate-errors-async-post