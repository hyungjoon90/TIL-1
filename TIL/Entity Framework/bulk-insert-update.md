# Entity FrameWork 대량 Insert/Update

서비스를 개발하다 보면 특정 조건을 만족하는 여러 개의 로우에 업데이트를 하거나, 여러 개의 로우를 추가하거나 삭제하는 것이 필요할 때가 있다.

AccountCarState Table

| seq | accountId | carId | state |
| --- | --- | --- | --- |
| `1` | `145` | `1` | `1` |
| `2` | `145` | `2` | `0` |
| `3` | `142` | `3` | `1` |
| `4` | `31` | `6` | `0` |
| `5` | `31` | `4` | `0` |
| `6` | `22` | `6` | `1` |

accountId는 AccountTable의 ID이며, state는 BIT컬럼이다.

예를 들어 위와 같은 다대다의 관계를 나타내는 테이블이 있을 때, AccountID컬럼이 145인 컬럼들의 carId값을 0로 업데이트 하겠다는 로직이 있다면, 쿼리를 사용하면

```UPDATE AccountCarState SET carId=0 WHERE accountId=145 ```

를 한다면 일괄로 업데이트를 할 수 있다.

하지만 ASP.NET 의 EF를 이용해서 위와 같은 작업을 하고 실제로 쿼리가 날아가는 것을 확인하면 굉장히 비효율적으로 업데이트를 진행한다.

```
public void CarUpdateByAccount(int accountId)
{
    var accountCarStateEntity = _db.AccountCarState.Where(x => x.accountId == accountId);
    foreach (var accountCarState in accountCarStateEntity)
    {
      accountCarState.carId = 1;
    }
    _db.SaveChanges();
}

```

위의 코드에서 _db는 DI를 통해 주입된 context라고 가정한다.

예제를 테스트하기 위한 메모장으로 작성한 코드이다. EF 에서는 이처럼 foreach 를 이용해서 코드를 작성해야 하는데

```_db.Database.Log = x => System.Diagnostics.Debug.WriteLine(x);```

를 이용해서 외부 툴을 이용하지 않고 실제로 쿼리가 날아가는 것을 확인할 수 있다.

그러면 업데이트 쿼리를 로우 당 하나씩 쿼리를 날리게 된다.

AccountCarState 에서 Where에 해당하는 조건을 가진 시퀸스를 n번 날린다고 생각하면 된다. 원시쿼리를 이용하면 한 줄에 끝나는 부분이 EF를 이용하면 비효율적인 방법을 사용하게 된다. 이 부분은 EF 깃허브의 이슈에 올라와있는 내용이며, 해결하기 위해서는 EF에서 원시쿼리를 지원하는데 그 기능을 이용하거나, EntityFramework.Extended 라는 외부 라이브러리를 이용하면 된다.

이것을 실제로 겪고 구글링을 열심히 한 결과 비슷한 내용의 글을 찾았었다.

---
#### 참고

https://www.seguetech.com/performing-bulk-updatesentity-framework-6-1/