# Nuxt.js Global CSS 적용

웹폰트를 `Nuxt.js` 전체 페이지에 적용 및 `Nuxt.js`에서 페이지 전환 효과에 관련된 부분들을 적용해 본다.

`assets/fonts/nanumsquareround.css` 생성

```css
@import url('https://cdn.rawgit.com/innks/NanumSquareRound/master/nanumsquareround.min.css');

body {
  font-family: 'NanumSquareRound', sans-serif;
}
```

`nuxt.config.js`를 수정
```javascript
  css: [
    '~/assets/css/transitions.css',
    '~/assets/fonts/nanumsquareround.css'
  ],
```

`'~/assets/css/transitions.css'`는 페이지전환으로 만들어놓은 `css`이다. 이런식으로 `Nuxt.js` 전역에 `css`적용이 가능하다.

`/assets/css/transitions.css`

```css
.page-enter-active,
.page-leave-active {
    transition: opacity .3s ease;
}
.page-enter,
.page-leave-to {
    opacity: 0;
}

.layout-enter-active,
.layout-leave-active {
    transition: opacity .3s
}
.layout-enter,
.layout-leave-active {
    opacity: 0;
}
```