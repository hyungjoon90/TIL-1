# Nuxt.js Components에서 props 사용

`props`를 사용할 때, `HTML`에서는 `kebab-case(article-data)`를 사용하는 것을 권장한다.

`Javascript`에서는 `camelCase(articleData)`를 사용하는 것을 권장한다.

```html
<article-card article-data="article"></article-card>
```

Nuxt.js 최신버전에서는 props을 아래와 같이 쓰는 것을 권장한다.

```javascript
export default {
  props: {
    articleData: {
      type: Array,
      default: []
    }
  }
}
```

---
#### 참고

https://beomy.tistory.com/56 

https://stackoverflow.com/questions/53659450/props-should-at-least-define-their-types