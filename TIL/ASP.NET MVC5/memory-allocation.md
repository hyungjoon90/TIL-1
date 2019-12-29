# .NET CLR에서 메모리 할당

```csharp
void CreateNote()
{
  TextBox myNote = new TextBox();
}
```

이 메서드는 `TextBox`의 `instance`를생성하고, 지역변수 `myNote`가 그것을 참조합니다.\
지역변수는 스택에 저장되지만, `instance` 자체는 힙에 저장이 됩니다.\
스택은 `reference-type`의 지역변수와 매개변수에 대한 참조자, 위에서 myNote를 가리키고 저장합니다.\
`value-type`의 지역변수와 메서드 매개변수등이 스택에 저장됩니다. `(struct, integer, char, DateTime)`

힙은 `referenct-type`의 `instance`가 포함하는 데이터, `instance` 내부에 구성된 모든것들을 저장합니다.

메모리 해제는 스코프를 벗어나면 스택에서 `pop`됩니다. 그리고 힙에 할당되어 있는것은
CLR의 가비지 콜렉터가 해당 `instance`에 대한 유효한 참조가 없다면 해제할 대상으로 보고 해제합니다.\
다만 이 `instance`를 바로 해제하지 않고, 힙메모리 공간이 부족하지 않는 상태가 된다면 그대로 유지합니다.\
만약 힙 메모리 공간이 부족하면 가비지 콜렉터가 유효한 참조가 없는 대상들을 찾아 해제합니다.

다만, 메모리를 제외한 자원인 `windows handle`, `file handle`, `SQL handle` 등은 할당한 `instance`가 필요없어졌을때 명시적으로 요청해야 합니다.

---
#### 참고

https://mooneegee.blogspot.com/2017/10/c-garbage-collection.html
