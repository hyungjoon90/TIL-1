```
    NProgress.configure({ showSpinner: false });
    NProgress.start();
    var interval = setInterval(function () { NProgress.inc(); }, 1000);

    $(window).on('load', function () {
        clearInterval(interval);
        NProgress.done();
    });

    $(window).on('unload', function () {
        NProgress.start();
    });
```

또는 ajax 전역 적용

```
    NProgress.configure({ showSpinner: false });

    $(document).ajaxStart(function () {
        NProgress.start();
    });

    $(document).ajaxSend(function () {
        NProgress.inc();
    });

    $(document).ajaxStop(function () {
        NProgress.done();
    });
    
    $(document).ajaxError(function () {
      NProgress.done();
    });
```

글로벌 ajax 옵션

https://api.jquery.com/category/ajax/global-ajax-event-handlers/

ajax 관련 참고 블로그

http://www.nextree.co.kr/p10308/