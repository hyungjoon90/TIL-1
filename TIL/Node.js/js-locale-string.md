# Javascript에서 TimeZone에 따른 시간 출력

`Node.js`에서 별다른 설정없이 `new Date()`로 저장을 하면 `DB`에 `UTC`시간이 저장된다. 이걸 혹시 사용자들에게 보여줄 때는 각 나라의 시간별로 맞게 바꾸어 주면 된다.

`utc`로 저장한 시간이 `datetime` 이라고 가정한다.

```javascript
getDate(datetime) {
  return new Date(datetime).toLocaleString('ko-KR', {
    timeZone: 'Asia/Seoul'
  })
}
```
위의 예시코드처럼 `timeZone`하고 출력 형식만 바꾸면 한국 시간 이외에 다른 나라 시간도 가능하다.