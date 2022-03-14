---
title: 참조한 FK Column이 터미널에서 식별되지 않는 점 및 Sequelize unknown column 에러에 관한 내용
author:
  name: SsankQ
date: 2022-03-05 21:30:00 +0800
categories: [EIF, First-Project]
tags: [First-Project, EIF]
render_with_liquid: false
---

### EIF

#### 어떤 에러인가?
- migration skeleton 파일을 생성하여 `addColumn`, `addConstraint`으로 컬럼 및 제약을 생성해주었고, mysql 상에서는 KEY - MUL 을 확인했으나 테이블에 `models.create`로 Insert할 때 FK로 설정된 user_id에 원하는 값이 Insert되지 않고 NULL 값만 출력, 터미널에서도 user_id 필드가 식별되지 않음

```js
const planData = await plans.create({ plan1, plan2, plan3, user_id: userId });
```

```bash
Executing (default): INSERT INTO `plans` (`id`,`plan1`,`plan2`,`plan3`,`createdAt`,`updatedAt`) VALUES (DEFAULT,?,?,?,?,?);
```

#### 에러 핸들링 방법

1. `models/index.js`에 직접 `sequelize.models` 를 불러와 관계에 따라 foreign key 참조하게 설정
  ```js
users.hasMany(plans, {
  foreignKey: "user_id"
});
plans.belongsTo(users, {
  foreignKey: "user_id"
});
  ```
2. migration 파일에 `addColumn` 부분을 지우고 `addConstraint`로 제약만 생성해준 것으로 **해결**
  ```js
  await queryInterface.addConstraint("plans", {
        fields: ["user_id"],
        type: "foreign Key",
        name: "FK_plans_users",
        references: {
          table: "users",
          field: "id",
        },
        onDelete: "cascade",
        onUpdate: "cascade",
  });
  ```
> ***모델 간의 관계를 설정해주면서 참조할 키를 지정해준 뒤, 관계에 대한 제약을 생성해주는 것이 올바른 방법인 듯하다***

#### 에러 및 원활히 진행되지 않던 점에 대한 추측

- 각 모델 파일 및 `index.js`에서 모델 간의 관계만 설정해주더라도 sequelize-cli 상에서 관계에 따라 
자동으로 column이 생성되었고, 터미널의 Executing 쿼리 부분에서 추가하지 않았던 userId가 비춰지는 것 같았다

> *관계 설정을 올바르게 하지않고, 참조할 키를 지정 안해주면 예상했던 거와는 다르게 unknown column이 생성되어 에러가 발생하는 듯 하다*

> ***즉, 자동으로 생성되는 userId와 같은 컬럼을 타겟으로 제약을 걸어주거나, 관계 설정할 때 참조할 키를 제대로 지정을 해준 뒤 제약을 걸어줘야 올바르게 FK 참조하는 관계 설정이 완료되는 듯하다. 이 부분은 추후 마저 알아볼 예정이다***

#### 에러 핸들링을 위해 참고한 레퍼런스 링크

[Agora states](https://github.com/codestates/agora-states/discussions/1329)
