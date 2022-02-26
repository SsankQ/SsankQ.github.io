---
title: EC2 인스턴스에서 서버가 실행되지 않는 문제
author:
  name: SsankQ
date: 2022-02-26 16:30:00 +0800
categories: [EIF, AWS]
tags: [AWS, EIF]
render_with_liquid: false
---

### EIF

*같이 프로젝트를 진행할 팀원께서 AWS를 이용한 배포를 연습하기 위해 EC2 인스턴스 상에서 서버를 실행하는 것을 연습하고 계셨는데 연습으로 사용할 Repository의 서버가 실행되지 않는 문제가 발생했다*

```bash
sh: 1: node: not found

npm ERR! Linux 4.4.0-1128-aws
npm ERR! argv "/usr/bin/nodejs" "/usr/bin/npm" "start"
npm ERR! node v4.2.6
npm ERR! npm v3.5.2
npm ERR! file sh
npm ERR! code ELIFECYCLE
npm ERR! errno ENOENT
npm ERR! syscall spawn
npm ERR! server@1.0.0 start: `node app.js`
npm ERR! spawn ENOENT
npm ERR! 
npm ERR! Failed at the server@1.0.0 start script 'node app.js'.
npm ERR! Make sure you have the latest version of node.js and npm installed.
npm ERR! If you do, this is most likely a problem with the server package,
npm ERR! not with npm itself.
npm ERR! Tell the author that this fails on your system:
npm ERR! node app.js
npm ERR! You can get information on how to open an issue for this project with:
npm ERR! npm bugs server
npm ERR! Or if that isn't available, you can get their info via:
npm ERR! npm owner ls server
npm ERR! There is likely additional logging output above.

npm ERR! Please include the following file with any support request:
npm ERR! /home/ubuntu/im-sprint-practice-dep
```

#### 문제 찾기

**에러 코드 확인** <br>

```bash
  npm ERR! node v4.2.6
  npm ERR! npm v3.5.2
```


*현 시점 기준으로 node, npm 버전이 낮은 걸 알게 되었고 이를 확인하려 했는데, <br> 이상하게도 **node 버전은 `v17.6.0`으로 확인**이 되었다*
*버전은 최신화되어 있었기에, 조금 더 하단을 살펴보니*


```bash
  npm ERR! code ELIFECYCLE
  npm ERR! errno ENOENT
```

*이러한 에러 코드를 출력하고 있었고, 바로 검색해보았다*  

---

#### 해결하기 위해 해본 것들

**1. 에러 코드 구글링**

```bash
  npm ERR! code ELIFECYCLE
  npm ERR! errno ENOENT
```

  <span style='color:gray'>*이 부분을 구글링 해본 결과, **쌓인 cache를 정리하고 기존 모듈과 `package.json`을 삭제한 뒤 `npm install & start`를 실행**해보라 하였다*</span>
  
  &nbsp;&nbsp;&nbsp;&nbsp;**a. 캐시정리**  
  : &nbsp;&nbsp;&nbsp;&nbsp;`npm cache clean --force`<br>

  &nbsp;&nbsp;&nbsp;&nbsp;**b. 기존 파일 삭제**
  : &nbsp;&nbsp;&nbsp;&nbsp;`rm -rf ./node_modules` <br> &nbsp;&nbsp;&nbsp;&nbsp;`rm -rf ./package-lock.json`  

  &nbsp;&nbsp;&nbsp;&nbsp;**c. npm 재설치 및 재시작**  
  : &nbsp;&nbsp;&nbsp;&nbsp;`npm install && npm start`

<span style='color:gray'>&nbsp;&nbsp;*이 방법을 진행하면서 <span style='color:black'>'EC2를 설치한 직후인데 쌓인 캐시로 문제가 발생하는게 맞을까'</span>하고 의문이 들었고 역시나 이 방법으로는 같은 에러를 출력하며 해결되지 않았다*</span>

**2. `node` 버전 최신화**  
  <span style='color:gray'>&nbsp;&nbsp;*인스턴스에는 `v17.6.0`으로 나와있었고, 내 로컬에서도 똑같이 진행해보았는데, 같은 버전으로 서버가 잘 실행되었다*</span>

  ![스크린샷, 2022-02-26 17-47-33](https://user-images.githubusercontent.com/89354370/155838516-292e4e59-ba0a-4cd8-b3eb-d6d18b0cc36d.png)


**3. `npm` 버전 최신화**  

  <span style='color:gray'>&nbsp;&nbsp;*이것저것 살펴보다 시간이 생각보다 지나버려서, 다시금 찬찬히 에러코드를 살펴보았다<br>
  &nbsp;&nbsp;**npm 버전에도 혹시 문제가 있나해서 이 부분도 조정하니 에러가 해결되며 서버가 잘 실행**되었다<br>
  &nbsp;&nbsp;&nbsp;&nbsp;(사실, 인스턴스 새로 설치하면서 처음부터 버전 잘 짚어보며 설치했다...)*</span>

*"위 방법 외에도 설치한 ubuntu 버전이 16.04이었기에 인스턴스 버전도 새로 맞춰서 생성해보기도 하고, <br>
node 버전을 lts로 조정도 해보고 이것저것 많이해보았지만 npm, node 버전 모두 체크해줘야 하는 것이 <br> 해결 방법이었다는 것을 조금 늦게 알게되었다.. <br>
사실 `npm ERR! Make sure you have the latest version of node.js and npm installed` 부분에서 <br> 
힌트는 주고 있었는데, 에러코드를 살펴보지않고, 바로 검색만 했던 것으로 시간이 훨씬 더 소비되었다"*
{: .text-center}

#### 발견점

***"에러도 꼼꼼히 살피는 것은 물론, 에러가 발생했다고 당황해서 기본적인 부분을 놓치고 있는 것이 아닌지 다시 한번 되짚어보는 습관을 들여야겠다"***

