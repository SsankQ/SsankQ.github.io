---
title: No Sequelize instance passed
author:
  name: SsankQ
date: 2022-03-03 18:30:00 +0800
categories: [EIF, First-Project]
tags: [First-Project, EIF]
render_with_liquid: false
---

### EIF

#### 어떤 에러인가?

- `sequelize-cli` 명령어를 통해 관계 설정에 따라 FK를 생성, 참조하고 난 후, 서버 실행하니 에러와 함께 서버 실행 실패

#### 에러 메시지

```bash
Error: No Sequelize instance passed
    at Function.init (/home/hyeongyu/codestates/project/33Plan/server/node_modules/sequelize/lib/model.js:659:13)
    at module.exports (/home/hyeongyu/codestates/project/33Plan/server/models/bads.js:18:8)
    at /home/hyeongyu/codestates/project/33Plan/server/models/index.js:24:54
    at Array.forEach (<anonymous>)
    at Object.<anonymous> (/home/hyeongyu/codestates/project/33Plan/server/models/index.js:23:4)
```

#### 에러 핸들링 방법

- 구글링했을 때 sequelize instance를 초기화하지 못한다는 말을 힌트 삼아 인스턴스 생성하는 각 `models.init`에 문제가 발생함을 발견

- 에러 원인
  - 테이블 간 관계를 정의하고 FK를 생성, 참조하기 위해 `init` 부분에 생성할 초기 Column을 모아놓은 객체를 삭제했던 것이 원인
  - `init(attributes: ModelAttributes<model, any>, options: InitOptions<model>): typeof model`
  - `attributes`에 필요한 인자를 지정해줘야하고 넣을 데이터가 없더라도 빈 객체를 할당해줬어야 한다
 
```jsx
// 에러 발생
good.init(
    {
      sequelize,
      modelName: "good",
    }
  );
  return good;

// 에러 해결
good.init(
    {},
    {
      sequelize,
      modelName: "good",
    }
  );
  return good;
```

#### 에러 핸들링을 위해 참고한 레퍼런스 링크

> *[Stack Overflow](https://stackoverflow.com/questions/70084090/error-no-sequelize-instance-passed-in-nodejs)*
