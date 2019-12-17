# C# 코드 스타일

### C# using문이 네임 스페이스 내부 or 외부

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
//using MyCorp.TheProduct;  <-- 주석 처리를 제거하면 아무것도 변경되지 않습니다
using MyCorp.TheProduct.OtherModule;
using MyCorp.TheProduct.OtherModule.Integration;
using ThirdParty;

namespace MyCorp.TheProduct.SomeModule.Utilities
{
    class C
    {
        Ambiguous a;
    }
}
```

```csharp
namespace MyCorp.TheProduct.SomeModule.Utilities
{
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using MyCorp.TheProduct;                           // MyCorp는 생략 할 수 있습니다. 이 사용은 중복되지 않습니다
    using MyCorp.TheProduct.OtherModule;               // MyCorp.TheProduct는 생략 될 수 있습니다
    using MyCorp.TheProduct.OtherModule.Integration;   // MyCorp.TheProduct는 생략 될 수 있습니다
    using ThirdParty;

    class C
    {
        Ambiguous a;
    }
}
```

관련된 문제 : https://stackoverflow.com/questions/125319/should-using-directives-be-inside-or-outside-the-namespace

[DotNetAnalyzers/StyleCopAnalyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers)를 사용하면 namespace 안에 using 문이 사용되어야 한다고 한다.

[dotnet/corefx](https://github.com/dotnet/corefx)의 소스를 보면 namespace를 외부에서 사용하고 있다.

### C# 코드 스타일

위에 namespace 관련 문제가 먼저 나온 이유는 관련된 문제 Stackoverflow에서도 그렇지만 사실 저 부분은 MS의 내부 코드 스타일에도 using은 namespace 내부에서 사용한다고 정의한다. 하지만 오래된 코드 스타일이고, 최근 MS의 오픈소스에 코드들을 보면 using은 대부분 namespace 밖에 있는 것 같다.

[editor Config](https://docs.microsoft.com/ko-kr/visualstudio/ide/create-portable-custom-editor-options?view=vs-2019)을 사용해서 코딩 컨벤션을 팀에 맞출 수 있다.

Visual Studio 2019에서 [Code Cleanup On Save](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.CodeCleanupOnSave)를
이용해서 자동 저장할 수 있다.

[dotnet/corefx의 editorconfig](https://github.com/dotnet/corefx/blob/master/.editorconfig)이다.