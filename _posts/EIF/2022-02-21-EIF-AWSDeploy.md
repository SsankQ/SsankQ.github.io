---
title: AWS Pipeline 진행 중 발생한 오류 정리
author:
  name: SsankQ
date: 2022-02-21 20:00:00 +0800
categories: [EIF, AWS]
tags: [AWS, EIF]
render_with_liquid: false
---

### EIF (Error I Faced)

*AWS, Github을 연결하여 서버 배포 자동화하는 실습 도중 발생한 오류를 정리해보았다*

#### <span style="color:red">***UnknownError***</span>

<span style="color:gray"> *"CodeDeploy agent was not able to receive the lifecycle event. Check the CodeDeploy agent logs on your host and make sure the agent is running and can connect to the CodeDeploy server"*</span>

![eventLog](https://user-images.githubusercontent.com/89354370/154988048-be990c96-2706-4db0-bf63-852ace34cfd0.png)

*<center>서버 코드 배포 도중 위와 같은 에러와 함께 배포에 실패하게 되었다. 이를 살펴보니</center>*

<img src='https://user-images.githubusercontent.com/89354370/154988652-0839802c-3490-42bf-b9d9-068db81eb5e5.png' alt='CheckLC' width=400px height=700px/>

*<center>다음과 같이 CodeDeploy LifeCycle이 시작도 하지 못한 부분을 확인 할 수 있었다 <br> 구글링을 통해서도 쉽게 찾아지지 않아 공식문서를 다시 한번 천천히 읽어보았다</center>*

<img src='https://user-images.githubusercontent.com/89354370/154989821-d926e7f0-b652-49a7-b264-6012d87f034c.png' alt='공식문서' width=700px height=600px/>

&nbsp;&nbsp;&nbsp;*빨간박스 부분을 읽는 순간 불현듯 이전 실습 때도 똑같은 옵션으로 깃헙 레포를 연결하여 생성한 애플리케이션이 생각났고 빠르게 애플리케이션 탭으로 가서 **해당 애플리케이션을 삭제하니 정상적으로 진행되었다.** <br> &nbsp;같이 학습한 동기 분들 중에서는 같은 방법으로 해결이 안됐다고 하시는 분도 있어 조금 더 면밀히 찾아봐야될 것 같다*

---

#### <span style="color:red">***Pipeline을 통해 생성되는 레포지토리 이름이 중복된 경우***</span>

<span style="color:gray"> *"The overall deployment failed because too many individual instances failed deployment, too few healthy instances are available for deployment, or some instances in your deployment group are experiencing problems"*</span>

*<center>UnknownError를 해결하고 배포를 재시도하는데 또 실패... 다시 한번 이벤트 로그를 확인해보니</center>*

![EventLog](https://user-images.githubusercontent.com/89354370/154992169-1723c874-9ce8-4ef6-a9da-50da80025f0a.png)

&nbsp;&nbsp;*다음과 같은 오류가 발생했고 이유는 배포에 사용되는 EC2 인스턴스에 있는 레포지토리 이름과 Pipeline을 통해 생성될 레포지토리 이름이 중복이 문제였다.
곧바로 EC2에 연결하여 **문제가 되는 레포지토리 이름을 수정해주었더니 바로 해결!***

---

#### <span style="color:red">***ScriptFailed***</span>

*앞서 설명한 오류들은 좀 부끄러운 케이스지만 이번에는 제대로 된(?) 오류코드와 함께 오류가 발생하여 내심 기뻤다..ㅋ*

<span style="color:gray"> *"The overall deployment failed because too many individual instances failed deployment, too few healthy instances are available for deployment, or some instances in your deployment group are experiencing problems"* </span>

*<center>아까도 같은 말이 나왔는데, <span style="color:red">"배포 그룹의 일부 인스턴스에 문제가 발생하여 전체 배포에 실패했다"</span>라고 한다</center>* <br> 
*<center>무튼 잘 좀 해보라는 뜻</center>*

![eventLog2](https://user-images.githubusercontent.com/89354370/154993810-faa98092-8d12-482d-a7a0-f4a110db1f3c.png)

*<center>어느 LifeCycle에서 문제가 발생했는지도 잘 짚어주는데 start.sh 쉘 스크립트 파일을 살펴봐도 잘못된 부분은 확인할 수 없었고, 이에 EC2 인스턴스에 기록된 로그를 확인해주었다</center>*

> CodeDeploy-Agent는 파이프라인 실행 때마다 로그를 해당 EC2 instance에 저장한다고 되어있다  
> `/opt/codedeploy-agent/deployment-root/deployment-logs` 경로에 저장되어 있는 것을 확인할 수 있었다

![deployLog](https://user-images.githubusercontent.com/89354370/154994929-9fe32a78-ae90-483d-92a1-7f466bb3af68.png)

*에러 로그를 확인해본 결과 EC2 인스턴스에 설치된 node, npm 버전이 현 시점과 비교해서 많이 낮은 걸 확인할 수 있었다 <br> (금일 기준 node lts 버전은 `v16.14.0`)*

![node,npm](https://user-images.githubusercontent.com/89354370/154995765-87f99dcc-94f7-4892-9571-810018543cbd.png)

***node 및 npm 버전을 최신화**해준 후 배포 재시도를 해보았다*
{: .text-center }

<img src='https://user-images.githubusercontent.com/89354370/154996078-d4a2bcb2-416b-41f6-bbc0-7d3ca8b9c176.png' alt='success' width=400px height=700px/>

***드디어 해결... 에러 로그 확인도 잘할 수 있어야 프로젝트, 현업 단계에서 당황하지 않고 천천히 해결할 수 있을 것 같다***
{: .text-center }