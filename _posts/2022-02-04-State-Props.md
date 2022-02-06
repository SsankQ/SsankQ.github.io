---
title: React State & props
author:
  name: SsankQ
date: 2022-02-04 15:00:00 +0800
categories: [React, React Intro]
tags: [React]
render_with_liquid: false
---

### React State & Props

<details>
<summary>Achievement Goals</summary>
<div markdown="1">
- [ ]  state, props의 개념에 대해서 이해하고, 실제 프로젝트에 바르게 적용할 수 있다
- [ ]  React Function Component에서 state hook을 이용하여 state를 정의 및 변경할 수 있다
- [ ]  React Component에 props를 전달할 수 있다
- [ ]  이벤트 핸들러 함수를 만들고 React에 이용할 수 있다
- [ ]  실제 웹 애플리케이션의 컴포넌트를 보고 어떤 데이터가 state고 props에 적합한지 판단할수 있다
- [ ]  실제 웹 애플리케이션 개발 시 적합한 state와 props의 위치를 스스로 정할 수 있다
- [ ]  React의 단방향 데이터 흐름(One-way data flow)에 대해 자신의 언어로 설명할 수 있다
</div>
</details>

### props란

- 컴포넌트의 속성을 의미  
: 변하지 않는 **외부로부터 전달받은 값** ⇒ 웹 앱에서 해당 컴포넌트가 가진 속성

- 부모 컴포넌트(상위)로부터 전달 받은 값

- **객체 형태** : 어떤 타입의 값도 전달할 수 있음

- **읽기 전용** : 함부로 변경되어서는 안되기 때문

> 만약 수정가능하게 되면 하위 컴포넌트에서 수정된 props가 상위에 영향을 끼치게 되고,  
이는 단방향, 하향식 데이터 흐름 원칙에 위배됨

---

#### How to use props 3단계

1. 하위 컴포넌트에 전달하고자 하는 값과 속성을 정의한다
2. props를 이용하여 정의된 값과 속성을 전달한다
3. 전달받은 props를 렌더링한다

```jsx
1. 각 컴포넌트를 선언, 작성
  function Parent() {
    return (
      <div className="parent">
        <h1>I'm the parent</h1>
        <Child />
      </div>
    );
  }

  function Child() {
    return (
      <div className="child"></div>
    );
  };

2. 전달하고자 하는 속성 및 값 할당
  <Child attribute={value} />

ex) <Child text={"I'm the eldest child"} />

3. 전달한 문자열을 Child 컴포넌트에서 받아오게 한다
// 함수에 인자를 전달하듯, 컴포넌트에 props 전달

function Child(props) {
  return (
    <div className="child"></div>
  );
};

4. props 렌더링 // JSX 문법 사용

function Child(props) {
  return (
    <div className="child">
      **<p>{props.text}</p>**  
    </div>
  );
}

// props는 객체 {attribute(=key): value} => props.text--
```

---

#### props.children

: props를 전달하는 또 다른 방법

```jsx
function Parent() {
  return (
    <div className="parent">
        <h1>I'm the parent</h1>
        **<Child>I'm the eldest child</Child>**
    </div>
  );
};

function Child(props) {
  return (
    <div className="child">
      **<p>{props.children}</p>**
    </div>
  );
};

// 여닫는 태그 사이에 value를 넣어 전달하는 방법도 가능
// 이 경우 props.children을 이용해 value에 접근
```

---

### State

**컴포넌트 내에서 변할 수 있는 값**

#### state hook, useState

- **useState 사용법**

```jsx
1. **React로부터 useState 불러오기**

import { useState } from "react";

2. **useState를 컴포넌트 안에서 호출**
   => state라는 변수를 선언하는 것과 같으며, 변수의 이름은 아무렇게 지어도 상관없음
  (*일반적인 변수는 함수가 끝날 때 사라지지만, state는 React에 의해 함수가 끝나도 안사라짐)

function CheckboxExample() {
  const [isChecked, setIsChecked] = useState(false);
// 새로운 state 변수를 선언

/* useState를 호출하면 배열을 반환 => r
   **배열의 0번째 요소는 현재 state 변수, 1번째 요소는 이 변수를 갱신할 수 있는 함수
   useState의 인자로 넘겨주는 값은 state의 초기값 */

  const [state 저장 변수, state 갱신 함수] = useState(상태 초기 값);**

// isChecked : state를 저장하는 변수
// setIsChecked : state **isChecked**를 변경하는 함수
// useState : state hook
// false : state 초기값

  이후 삼항연산자로 true false에 따라 결과 여부
  <span>{isCheckd ? "Checked!!" : "Unchecked"}</span>
```

---

#### state 갱신하기

- state를 갱신하려면 state 변수를 갱신할 수 있는 함수를 호출
- 이벤트 핸들러

```jsx
function CheckboxExample () {
  const [isChecked, setIsChecked] = useState(false);

  const handleChecked = (event) => {
    setIsChecked(event.target.checked);
  };

  return (
    <div className="App">
      <input type="checkbox" checked={isChecked} onChange={handleChecked} />
      <span>{isChecked ? "Checked!!" : "Unchecked"}</span>
    </div>
  );
}
```

> ***주의점***
{: .prompt-warning }
- React 컴포넌트는 state가 변경되면 새롭게 호출되고, 리렌더링됨
- React state는 상태 변경 함수 호출로 변경해야 함 ***(강제 변경 시도는 절대 X)***

  *⇒ 그렇지 않으면 리렌더링이 되지않거나, state가 제대로 변경되지 않음*

---

### 이벤트 처리

- React에서 이벤트는 소문자 대신 *카멜 케이스*를 사용
- JSX를 사용하여 **문자열이 아닌 함수**로 이벤트 처리 함수(Event handler)를 전달

```jsx
// HTML에서의 이벤트 처리 방식
<button onclick="handleEvent()">Event</button>

// React에서의 이벤트 처리 방식
<button onClick={handleEvent}>Event</button>
```

- onChange

```jsx
function NameForm() {
	const [name, setName] = useState('');
  const handleChange = (e) => {
    setName(e.target.value);
  }

  return (
    <div>
      <input type="text" value={name} onChange={handleChange}></input>
      <h1>{name}</h1>
    </div>
  )
};

/* 1. onChange 이벤트가 발생하면 e.target.value를 통해 이벤트 객체에 담겨있는 input값을 읽음
     (**onChange는 input의 텍스트가 바뀔때 마다 발생하는 이벤트**)
 * 2. 이벤트 발생하면 handleChange 함수 작동
 * 3. 이벤트 객체에 담긴 input값을 setState를 통해 새로운 state로 갱신 */
```

- onClick

```jsx
function NameForm() {
  const [name, setName] = useState('');

  const handleChange = (e) => {
    setName(e.target.value);
  }

  return (
    <div>
      <input type="text" value={name} onChange={handleChange}></input>
      <button onClick={alert(name)}>Button</button>
      <h1>name</h1>
    </div>
  );
};

/* 1. button을 클릭하면 onClick 이벤트 발생 => alert 함수 호출
 * 2. 함수 자체가 아닌 함수의 값이 호출됨 : alert**(name)
 ***** 함수에 리턴값이 존재하지 않기에 undefined 반환 => onClick에 적용 => **클릭 시 변화 x** ***/
```

- onClick - 함수 정의하기, 함수 자체 전달하기

```jsx
// 1. 함수 정의하기
  return (
    <div>
      <button onClick={ () => alert(name)}>Button</button>
    </div>
    );
  };

// 2. 함수 자체 전달하기
  const handleClick = () => {
    alert(name);
  };

  return (
    <div>
      <button onClick={handleClick}>Button</button>
    </div>
    );
```