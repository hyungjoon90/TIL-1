# CloudFront가 오래된 객체를 제공하는데 해결 방법

CloudFront를 사용하는데 S3에 새롭게 객체를 업로드 하여도 이전의 객체가 나오는 경우가 있습니다. S3 객체가 CloudFront에 업데이트되지 않는 이유는 CloudFront에서 캐시하기 때문입니다.

이를 해결하려면 객체의 버전을 관리하거나, S3객체를 무효화 해야 합니다.

객체의 버전을 관리하는 방법은 ?v=1, ?ver=2, ?v=hash 이런 식으로 파일의 이름을 다르게 하면 새로운 객체이기 때문에 뒤에 버전을 붙여서 관리합니다.

S3객체를 무효로 하는 방법입니다. 아마존에서 aws cli를 설치

실행 후 ```aws --version```

설치가 잘되어있는지 확인합니다.

aws s3에 엑세스하기 위해서 아래의 명령어 입력합니다.

### s3에 엑세스하기 위한 정보 등록

```aws configure```

입력해서 엑세스키와 시크릿엑세스키 입력, 리전 ap-northeast-2 를 입력한다.
그리고 마지막 format은 입력하지 않고 그냥 넘어간다.

그다음 aws s3 ls 로 버킷리스트를 확인할 수 있다.

### 내 계정의 버킷 리스트

```aws s3 ls```

### 내 버킷 내 파일 리스트

```aws s3 ls s3://mybucket/```

### 등록한 AWS Access Key ID와 Secret Access Key 확인

```aws configure list```

### 캐시무효화(ex : main.css 파일)

```aws cloudfront create-invalidation --distribution-id $CDN_DISTRIBUTION_ID--paths "/live/cdn/css/main.css"```

### 캐시무효화(ex : css 폴더)

```aws cloudfront create-invalidation --distribution-id $CDN_DISTRIBUTION_ID--paths "/live/cdn/css/*"```

### 캐시무효화 결과값 요청

```aws cloudfront get-invalidation --distribution-id ${CDN_DISTRIBUTION_ID} --id ${결과 ID}```

---
#### 참고

https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html#Invalidation_Expiration