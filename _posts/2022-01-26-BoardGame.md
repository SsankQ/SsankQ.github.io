---
title: 보드게임
author:
  name: SsankQ
date: 2022-01-26 19:00:00 +0800
categories: [Algorithms & CodingTest, CodingTest]
tags: []
render_with_liquid: false
---

```jsx
/*
 ? N * N의 크기를 가진 보드판 위에서 게임을 하려고 합니다. 게임의 룰은 다음과 같습니다.
 1. 좌표 왼쪽 상단(0, 0)에 말을 놓는다.
 2. 말은 상, 하, 좌, 우로 이동할 수 있고, 플레이어가 조작할 수 있다.
 3. 조작의 기회는 딱 한 번 주어진다.
 4. 조작할 때 U, D, L, R은 각각 상, 하, 좌, 우를 의미하며 한 줄에 띄어쓰기 없이 써야 한다.
 5. 한 번 움직일 때마다 한 칸씩 움직이게 되며, 그 칸 안의 요소인 숫자를 획득할 수 있다.
 6. 방문한 곳을 또 방문해도 숫자를 획득할 수 있다.
 7. 보드 밖을 나간 말은 OUT 처리가 된다.
 8. 칸 안의 숫자는 0 또는 1이다.
 9. 단, 좌표 왼쪽 상단(0, 0)은 항상 0이다.
 * 획득한 숫자를 합산하여 숫자가 제일 큰 사람이 이기게 된다.
 * 보드판이 담긴 board와 조작하려고 하는 문자열 operation이 주어질 때, 말이 해당 칸을 지나가면서 획득한 숫자의 합을 구하는 함수를 작성하세요.
 ! 만약, 말이 보드 밖으로 나갔다면 즉시 OUT 을 반환
  인자 1: board - number 타입의 2차원 배열 / 2 <= board.length <= 1,000
  인자 2: operation - string 타입의 대문자 영어가 쓰여진 문자열 / 1 <= operation.length <= board.length * 2
  U, L, D, R 이외의 문자열은 없습니다.
 */

function boardGame(board, operation) {
    const DIR = {
        'U': [-1, 0],
        'D': [1, 0],
        'L': [0, -1],
        'R': [0, 1]
    }
    const N = board.length
    const isValid = (y, x) => y >= 0 && x >= 0 && y < N && x < N;
    let Y = 0, X = 0, score = 0;

    for(let i = 0; i < operation.length; i++) {
      const [dY, dX] = DIR[operation[i]];
      Y += dY;
      X += dX;
      if(!isValid(Y, X)) return 'OUT';
      score += board[Y][X];
    }

    return score;
}

const board1 = [
    [0, 0, 0, 1],
    [1, 1, 1, 0],
    [1, 1, 0, 0],
    [0, 0, 0, 0]
  ]
const operation1 = 'RRDLLD'
const output1 = boardGame(board1, operation1);

console.table(board1);
console.log(`위 보드에서 말을 '${operation1}'으로 조작해 획득한 점수는 ${output1}점 입니다`)
```