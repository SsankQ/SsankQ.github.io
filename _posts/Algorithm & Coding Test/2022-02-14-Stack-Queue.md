---
title: Stack & Queue
author:
  name: SsankQ
date: 2022-02-14 11:00:00 +0800
categories: [Algorithms & CodingTest, Algorithms]
tags: [Stack, Queue]
render_with_liquid: false
---

### 자료구조

- 데이터 : 문자, 숫자, 소리, 그림, 영상 등 실생활을 구성하고 있는 모든 값

  ⇒ 데이터는 분석하고 정리하여 활용해야만 의미를 가질 수 있음  
  ⇒ 필요한 데이터에 따라 데이터의 특징을 잘 파악(분석)하여 정리하고, 활용해야 함  
  ⇒ 데이터를 체계적으로 정리하여 저장해두는 게, 활용하는 데에 있어 훨씬 유리  

- **자료구조** : 데이터를 효율적으로 다룰 수 있는 방법을 모아둔 것

![자료구조](https://user-images.githubusercontent.com/89354370/153799338-f83b6db0-c2b3-4f20-89a6-4f276bcb557f.png)

> 가장 자주 등장하는 4가지 ⇒ **Stack, Queue, Tree, Graph**  
: 각각 특정 상황에 놓인 문제를 해결하는 데에 특화되어 있음 ⇒ 많은 자료구조를 알아두어야 다양한 상황에 대처하여 해결할 수 있다

---

#### Stack 
*데이터를 순서대로 쌓는 구조*

- 입력과 출력이 하나의 방향으로 이루어지는 **제한적 접근**  
⇒ 가장 먼저 들어간 데이터가 가장 나중에 나올 수 있음

- 위의 자료구조의 정책: **LIFO(Last In First Out) / FILO (First In Last Out)**

<img src='https://user-images.githubusercontent.com/89354370/153799560-390517ee-6be8-428b-9fef-0f3f846a96ca.png' alt='stack' width=600px height=400px />
*그림 출처 :  https://velog.io/@jiwon22/20210414*


- 실사용 예제 : 브라우저의 뒤로 가기, 앞으로 가기 기능

```jsx
1. 새로운 페이지로 접속할 때, 현재 페이지를 Prev Stack에 보관한다
2. 뒤로 가기 버튼을 눌러 이전 페이지로 돌아갈 때에는, 현재 페이지를 Next Stack에 보관하고,
   **Prev Stack에 가장 나중에 보관된 페이지**를 현재 페이지로 가져온다
3. 앞으로 가기 버튼을 눌러 앞서 방문한 페이지로 이동할 때에는, **Next Stack의 가장 나중에 보관된
   페이지**를 가져온다
4. 마지막으로 현재 페이지를 Prev Stack에 보관한다
```

---

#### Queue 
*데이터가 입력된 순서대로 처리될 때 주로 사용*

- Stack과는 정반대되는 개념
- **FIFO (First In First Out) / LILO (Last In Last Out)**

  ⇒ 가장 먼저 들어간 데이터가 먼저 나오게되는 데이터 구조 ex)프린터 인쇄 순서, 톨게이트

- 실사용 예제 : 프린터

```jsx
1. 문서를 작성하고 출력 버튼을 누르면 해당 문서는 인쇄 작업[임시 기억 장치의] Queue에 들어감
2. 프린터는 인쇄 작업 Queue에 들어온 문서를 순서대로 인쇄

// (출력 버튼) - (임시 기억 장치의) Queue에 하나씩 들어옴 - Queue에 들어온 문서 순서대로 인쇄

// 아래의 buffer 개념을 접목하여 다시 정리하면
- 일반적으로 프린터는 속도가 느림
- CPU는 프린터와 비교하여, 데이터 처리 속도가 빠름
- **CPU는 빠른 속도로 인쇄에 필요한 데이터를 만든 다음, 인쇄 작업 Queue에 저장하고 다른 작업 수행**
- **프린터는 인쇄 작업 Queue에서 데이터를 받아 일정한 속도로 인쇄** 

=> 동영상 스트리밍에서 버퍼링 발생하는 이유도 위와 같음
```

- 위 예시처럼 컴퓨터 장치들 사이에서 데이터를 주고 받을 때, **각 장치 사이에 존재하는**

  **속도의 차이나 시간 차이를 극복하기 위해 임시 기억 장치의 자료구조로 Queue를 사용**

  ⇒ **이를 통틀어 버퍼(Buffer)라고 부름**

- **버퍼링(Buffering)**

    - 대부분의 컴퓨터 장치에서 발생하는 이벤트는 파동 그래프와 같이 불규칙적  
    - 이에 비해 CPU와 같이 이벤트를 처리하는 장치는 일정한 처리 속도를 가짐  
    **불규칙적으로 발생한 이벤트를 규칙적으로 처리하기 위해 버퍼(Buffer)를 사용**
<img src='https://user-images.githubusercontent.com/89354370/153799750-780a0c5e-90f2-431b-befa-81039a0dc7ac.png' alt='버퍼링' width=500px height=400px  />


---