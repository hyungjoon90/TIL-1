# CloudFront CORS policy 오리진 오류 해결 방법

외부에서 웹 폰트를 다운로드해서 불러올 때 가장 많이 발생합니다. CloudFront 오리진 관련 오류로서 CORS 구성을 허용해주어야 합니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>https://허용할도메인.com</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>Content-*</AllowedHeader>
        <AllowedHeader>Host</AllowedHeader>
    </CORSRule>
    <CORSRule>
        <AllowedOrigin>https://*.허용할도메인.com</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>Content-*</AllowedHeader>
        <AllowedHeader>Host</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

---
#### 참고

https://icon.town/icon/23554