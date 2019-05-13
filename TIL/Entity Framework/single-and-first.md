# SingleOrDefault 와 FirstOrDefault 의 차이

#### 결과 레코드가 많은 경우에
SingleOrDefault는 예외를 던집니다.
FirstOrDefault는 첫 번째 레코드를 반환합니다.

#### 결과 레코드가 1개라면
SingleOrDefault는 그 레코드를 반환합니다.
FirstOrDefault는 그 레코드를 반환합니다.

#### 결과 레코드가 없다면
SingleOrDefault는 유형의 기본값을 반환합니다 (예 : int의 기본값은 0 임).
FirstOrDefault는 형식의 기본값을 반환합니다.

#### first 와 take 의 차이
first() 정확히 하나의 요소를 반환한다는 것을 의미합니다. 컬렉션의 첫 번째 요소입니다.
take(x) 그것은 요소들의 컬렉션을 리턴할 것을 암시한다. x 컬렉션의 첫 번째 요소입니다.
