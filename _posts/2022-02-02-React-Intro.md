---
title: React Intro
author:
  name: SsankQ
date: 2022-01-28 15:00:00 +0800
categories: [React, React Intro]
tags: [React]
render_with_liquid: false
---

### React Intro

<details>
<summary>Achievement Goals</summary>
<div markdown="1">
- [ ]  React의 3가지 특성을 이해하고, 설명할 수 있다
- [ ]  JSX가 왜 명시적인지 이해하고, 바르게 작성할 수 있다
- [ ]  React Component의 필요성에 대해서 이해하고, 설명할 수 있다
- [ ]  create-react-app 으로 간단한 개발용 React 앱을 실행할 수 있다
</div>
</details>

---

***React란?***  
:  프론트엔드 개발을 위한 JavaScript 오픈소스 라이브러리

#### 리액트의 3가지 특징

- **선언형 ( Declarative )**  
: 한 페이지를 보여주기 위해 HTML / CSS / JS로 나눠 적기보다 하나의 파일에  
**명시적**으로 작성할 수 있게 JSX를 활용한 선언형 프로그래밍을 지향  

> 명시적 : 코드를 자세히 분석하지 않고도, 코드의 의도를 분명히 알 수 있게 작동하는 방식

---

- **컴포넌트 기반 ( Component Based )**
    
    - 컴포넌트 : 하나의 기능 구현을 위해 여러 종류의 코드를 하나로 묶어둔 것
    
    - 컴포넌트로 분리하면 서로 독립적이고 재사용 가능하기 때문에, 기능 자체에 집중하여 개발 가능하며  
    유지 보수, 유닛 테스트에도 편리함
    
    > 컴포넌트 (Component) : 구조와 동작에 대한 코드를 한 뭉치로 적은 코드셋
    
    ![컴포넌트](https://user-images.githubusercontent.com/89354370/152316148-8812ec6c-26fd-4250-8f73-62fe0bbb5f23.png)
    
    ---
    

- **범용성 ( Learn Once, Write Anywhere )**
    
    - JavaScript 프로젝트 어디서든 유연하게 사용할 수 있다
    
    ---
    

#### JSX (JavaScript XML)
> UI를 구성할 때 사용가능하게 JavaScript를 확장한 문법

- 브라우저가 바로 실행할 수 있는 코드는 아님 ⇒ JavaScript 코드로 변환 ***(Babel)***
    
    > Babel : JSX를 브라우저가 이해할 수 있는 JavaScript로 컴파일하는 것
    
    - **JSX는 HTML이 아니기 때문에 컴파일 필수** 
    - JSX를 사용함으로써 코드의 복잡성을 줄이고, 이를 이해하기 쉽게 만들 수 있음
    
    ![JSX](https://user-images.githubusercontent.com/89354370/152316700-1b3c8a40-dafb-4faa-92c5-641761441b6c.png)
    
    ---
    

##### JSX 문법과 규칙

- Opening tag와 Closing tag로 감싸주어야 함 **(하나의 엘리먼트 안에 모든 엘리먼트 포함)**

    ```jsx
    <div>
        <div>
            <h1>Hello</h1>
        </div>
        <div>
            <h2>World</h2>
        </div>
    </div>
    ```

- 엘리먼트 클래스 사용 시, **className으로 표기**

    ```jsx
    <div class = "" > X

    <div className="greeting"> Hello! </div>
    ```

- JavaScript 표현식 사용 시, **중괄호** 이용 ( 사용하지 않으면 일반 텍스트로 인식 )

    ```jsx
    function App() {
        const name = 'Josh';

        return (
            <div>
                Hello, **{name}** // 반드시 {} 사용
            </div>
        );
    }
    ```

- **사용자 정의 컴포넌트는 대문자로 시작 ( PascalCase )**

    ```jsx
        function Hello() {
            return <div>Hello!</div>;
        }

        function HelloWorld() {
            return <Hello />;
        }
    ```

- 조건부 렌더링에서는 **삼항연산자 사용**

    ```jsx
        <div>
          {
            (1+1 === 2) ? (<p>정답</p>) : (<p>탈락</p>)
          }
        </div>
    ```

- 여러개의 HTML 엘리먼트를 표시할 때, **map() 함수 사용**

    ```jsx
        const posts = { *** };

        function Blog() {
            const content = posts.map((post) =>
            <div key={post.id}>  // **map 사용시 반드시 "key" JSX 속성을 넣어야함**
                <h3>{post.title}</h3>
                <p>{post.content}</p>
            </div>
        );

        return (
            <div>
                {content}
            </div>
        );
        }
    ```

---

##### map을 이용한 반복

```jsx
// 배열의 각 요소(post)를 특정 논리(postToElement)에 의해 다른 요소로 지정
1)
function Blog () {
  const postToElement = (post) => {
    <div>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );

  const blogs = posts.map(postToElement);

  return <div className="post-wrapper">{blogs}</div>;
}

/* return 문 안에서 map 메소드를 사용할 수는 없을까요?
사용할 수 있습니다. JSX를 사용하면 중괄호 안에 모든 표현식을 포함할 수 있기 때문에
map 메소드의 결과를 return문 안에 인라인으로 처리할 수 있습니다. 
코드 가독성을 위해 변수로 추출할지 아니면 인라인에 넣을지는 개발자가 판단해야 할 몫입니다.
*/
```

- Key 속성

```jsx
/*
key 속성값은 가능하면 데이터에서 제공하는 id를 할당해야 합니다. 
key 속성값은 id와 마찬가지로 변하지 않고, 예상 가능하며, 유일해야 하기 때문입니다. 
고유한 id가 없는 경우에만 배열 인덱스를 넣어서 해결할 수 있습니다
*/

function Blog() {
  const blogs = posts.map((post) => {
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  });
  return <div className="post-wrapper">{blogs}</div>;
}   
```