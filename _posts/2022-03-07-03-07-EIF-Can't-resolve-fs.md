---
title: Module not found - Error Can't resolve 'fs' , 'net'
author:
  name: SsankQ
date: 2022-03-07 22:20:00 +0800
categories: [EIF, First-Project]
tags: [First-Project, EIF]
render_with_liquid: false
---

### EIF

#### 어떤 에러인가?

- S3 정적 웹 페이지 호스팅 단계에서 버킷에 넣어줄 빌드 파일을 생성하기 위해 `npm run build`를 실행했으나, 
 에러가 발생하며 진행되지 않음

#### 에러 메시지

```bash
Module not found: Error: Can't resolve 'fs' , 'net' in [파일 경로]
```

위와 같은 에러 메시지가 출력되어 찾아보니 공식문서에서 다음과 같은 부분을 볼 수 있었다

![image](https://user-images.githubusercontent.com/89354370/157068509-f6bdefdd-5fb0-4325-b415-107f67a3a5e2.png)

*참조한 내용으로 보아 `webpack 5`부터는 `fs`, `net` 등의 핵심 Node.js 모듈을 자동으로 **폴리필**을 지원하지 않으므로 필요한 모듈은 직접 npm을 통해 설치하여 사용하라는 뜻으로 보인다*

> ***폴리필(polyfill)**
> 웹 개발에서 기능을 지원하지 않는 웹 브라우저 상의 기능을 구현하는 코드를 뜻한다*

#### 에러 핸들링 방법

**!! 작업 중인 폴더 및 파일을 보존하고 싶으시면 복사를 하는 등의 방법으로 진행하시길 권장드립니다**

*사실 현걸님 로컬에서는 잘 빌드되지 않았는데 내 로컬에서는 빌드되어 빌드파일을 압축해서 드렸다...*

1. **npm run eject** 명령어 실행
 
> **`npm run eject`**  
> CRA(Create-React-App)에서 위 명령어를 실행하면 숨겨진 파일까지 모두 꺼내 파일을 직접 수정, 조회할 수 있게 된다
> **단, 한번 명령어를 실행하게 되면 취소가 불가능해지게 되니 필요할 때에만 신중히 진행할 것!**
  
2. `Are you sure you want to eject? This action is permanent.(y/N)` 와 같은 메시지가 출력된다.
 
![image](https://user-images.githubusercontent.com/89354370/157070823-e181cc11-5fd5-476d-8a96-36613d7184c8.png)
*수락하면 다음과 같이 숨어서 리액트에 대한 기본적인 기능(Scripts 실행 및 webpack 등등)에 대한 파일들이 쭉 나타나게 된다*

3. 생성된 파일 중  `webpack.config.js` 파일에 다음과 같은 내용을 추가해주면 된다

```js
node : {
  'fs': empty,
  'net': empty,
}
```

4. 수정이 끝난 후에는 필요한 작업을 진행 후 생성된 `config, scripts, node_modules` 등 추가 및 변경된 사항을 모두 삭제하고 다시 `npm install`로 다시 필요한 모듈을 받아준다

#### 에러 핸들링을 위해 참고한 레퍼런스 링크

[Webpack 공식문서](https://webpack.js.org/configuration/resolve/#resolvefallback)
[참조 사이트](https://exerror.com/module-not-found-error-cant-resolve-fs-in/)
