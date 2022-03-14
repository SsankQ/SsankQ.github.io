---
title: ERR HTTP HEADERS SENT
author:
  name: SsankQ
date: 2022-03-04 19:30:00 +0800
categories: [EIF, First-Project]
tags: [First-Project, EIF]
render_with_liquid: false
---

### EIF

#### 어떤 에러인가?

- 서버가 클라이언트에 응답을 둘 이상 돌려줄 때 발생한 오류
- 이미 응답을 한번 했음에도 추가적인 응답을 돌려줄 때 발생

#### 에러 메시지

```bash
[ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client
```

#### 에러 핸들링 방법

- 한번의 요청에 둘 이상의 응답이 발생하지 않게 조건에 맞춰 올바르게 `return` 작성

```js
if (password) {
  if (!validatePW(password)) {
    return res.status().send()
  }
}

res.status().send()
```
