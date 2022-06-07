---
title: CloudFront - AccessDeniedException / CodePipeline - AccountLimitExceededException
author:
  name: SsankQ
date: 2022-05-10 21:00:00 +0900
categories: [EIF, Side-Project]
tags: [Side-Project, EIF]
render_with_liquid: false
---

### 어떤 에러인가?

- 외구님 AWS Console에서 테스트 배포 진행 중 `CloudFront`가 생성되지 않아 구글링을 통해 방법을 찾는데 꽤 시간이 소요됨
- 마찬가지로, `CodePipeline`에서 배포 자동화 파이프라인을 생성 후 Build 단계에서 `AccountLimitExceededException` 오류 발생하여 빌드가 진행되지 않음
 
 *AWS 계정 자체에 limit가 낮게 설정되어 있는지, 아니면 IAM 보안 관련 이슈인지는 AWS 회신을 통해 확인 후 추가적으로 알아볼 예정이다*

### 에러 메시지

- CloudFront
```
AccessDeniedException: Your account must be verified before you can add new CloudFront resources. To verify your account, please contact AWS Support (https://console.aws.amazon.com/support/home#/ ) and include this error message. (Service: AmazonCloudFront; Status Code: 403; Error Code: AccessDenied; Request ID: ~~~)
```

- CodePipeline - Build Stage
```
Error calling startBuild: Cannot have more than 0 active builds for the account (Service: AWSCodeBuild; Status Code: 400; Error Code: AccountLimitExceededException; Request ID: <x>)
```

### 에러 핸들링 방법

- CloudFront Distributions limit를 추가할 수 있게 AWS Service Quotas에 할당량 증가 요청
- Build Stage도 마찬가지로 AWS Service Quotas를 통해 할당량 증가 요청

### 에러 핸들링을 위해 참고한 레퍼런스 링크

- CloudFront
  - [AWS Report](https://repost.aws/questions/QULXHEAzC7Sai6_LTLNYn83Q?threadID=307967)
- CodeBuild
  - [MI-NE 님 블로그](https://minemanemo.tistory.com/158) 