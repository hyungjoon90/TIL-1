# Nuxt.js에서 무한 스크롤링

설치
```
npm install vue-infinite-loading -S
```

`plugins/vue-infinite-loading.js` 생성
```javascript
import Vue from 'vue';
import InfiniteLoading from 'vue-infinite-loading';

Vue.use(InfiniteLoading, { /* options */ });
```

`nuxt.config.js` 수정
```javascript
module.exports = {
  mode: 'universal',
  // ... 중략
  plugins: [
    { src: '~/plugins/vue-infinite-loading.js', mode: 'client' }
  ],
  // ... 중략
}
```

전역에서 사용가능하기 때문에 사용을 원하는 템플릿에서 사용
```javascript
    <infinite-loading @infinite="infiniteHandler" spinner="spiral">
      <div slot="no-more">No more message</div>
      <div slot="no-results">No results message</div>
      <div slot="error" slot-scope="{ trigger }">
        Error message, click
        <a href="javascript:;" @click="trigger">here</a> to retry
      </div>
    </infinite-loading>
```

```javascript
export default {
  methods: {
    infiniteHandler($state) {
      setTimeout(() => {
        this.datas.push(...sampleData);
        // this.datas = [...this.datas, ...sampleData];
        $state.loaded();
      }, 1000);
    }
  }
};
```
`spinner="spiral"` 를 이용해서 `infitite-loading` 모듈에서 제공해주는 `loading spinner` 사용 가능\
`loading spinner` 테스트를 위한 `setTimeout`

---
#### 참고

https://peachscript.github.io/vue-infinite-loading/guide/
