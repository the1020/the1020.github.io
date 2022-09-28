---
title: 최소힙으로 우선순위 큐 구현하기
author: 예은
date: 2022-09-28 20:55:00 +0900
categories: [study]
tags: [자료구조, 힙, 우선순위큐]
subtitle:
  [
    우선순위 큐는 각 요소가 그에 해당하는 우선순위를 가지는 데이터 구조이다. 더 높은 우선순위를 가진 요소가 먼저 처리된다.,
  ]
---

> ✍️ [JavaScript 알고리즘 & 자료구조 마스터클래스](https://www.udemy.com/course/best-javascript-data-structures/) 강의를 보면서 정리하는 글

## 우선순위 큐(Priority Queue)

우선순위 큐는 각 요소가 그에 해당하는 우선순위를 가지는 데이터 구조이다. 더 높은 우선순위를 가진 요소가 먼저 처리된다.

## 최소 힙으로 우선순위 큐 구현하기

> [최대힙](/posts/data-structure-heap/#구현)을 최소힙으로 변경하고 비교문을 **노드 자체가 아닌 우선순위 프로퍼티끼리 비교**하도록 수정해주면 된다.

- 낮은 숫자는 우선순위가 높음을 의미하므로 최소 힙으로 구현하도록 한다.
- 각 요소는 값과 우선순위를 저장할 객체 단위로 요소를 관리한다.

```javascript
class Node {
  constructor(value, priority) {
    this.value = value;
    this.priority = priority;
  }
}

class PriorityQueue {
  constructor() {
    this.list = [null];
  }

  // 최대 힙에서 만든 swap 메서드 그대로 사용하기
  swap(a, b) {
    [this.list[a], this.list[b]] = [this.list[b], this.list[a]];
  }
}
```

### ⛳️ Enqueue

```javascript
class PriorityQueue {
  enqueue(value, priority) {
    // ✅ 노드는 값과 우선순위를 담는 node 인스턴스로 구성된다.
    const newNode = new Node(value, priority);

    this.list.push(newNode);

    let curIdx = this.list.length - 1;
    let parIdx = Math.floor(curIdx / 2);

    // ✅ 부모와 자식 간 값 비교는 요소 자체가 아닌 요소의 우선순위 프로퍼티끼리 비교한다.
    while (
      curIdx > 1 &&
      this.list[curIdx].priority < this.list[parIdx].priority
    ) {
      this.swap(curIdx, parIdx);

      curIdx = parIdx;
      parIdx = Math.floor(curIdx / 2);
    }
  }
}
```

### 🗑 Dequeue

```javascript
class PriorityQueue {
  dequeue() {
    const min = this.list[1];

    if (this.list.length <= 2) {
      this.list = [null];
      return min;
    }

    this.list[1] = this.list.pop();

    let curIdx = 1;
    let leftIdx = curIdx * 2;
    let rightIdx = curIdx * 2 + 1;

    if (!this.list[leftIdx]) {
      return min;
    } else if (!this.list[rightIdx]) {
      // ✅ 부모와 자식 간 값 비교는 요소 자체가 아닌 요소의 우선순위 프로퍼티끼리 비교한다.
      if (this.list[leftIdx].priority < this.list[curIdx].priority) {
        this.swap(leftIdx, curIdx);
      }
      return min;
    }

    // ✅🌟 자식 노드는 존재하지 않을 수도 있다. 노드가 존재하지 않는데 priority라는 속성을 불러오면 에러가 발생한다.
    // 자바스크립트의 옵셔널 체이닝 ?.을 사용하여 오류를 방지한다.
    while (
      this.list[leftIdx]?.priority < this.list[curIdx].priority ||
      this.list[rightIdx]?.priority < this.list[curIdx].priority
    ) {
      // ✅ 노드 비교는 요소 자체가 아닌 요소의 우선순위 프로퍼티끼리 비교한다.
      const minIdx =
        this.list[leftIdx].priority > this.list[rightIdx].priority
          ? rightIdx
          : leftIdx;

      // ✅ 부모와 자식 간 값 비교는 요소 자체가 아닌 요소의 우선순위 프로퍼티끼리 비교한다.
      if (this.list[minIdx].priority < this.list[curIdx].priority) {
        this.swap(minIdx, curIdx);

        curIdx = minIdx;
        leftIdx = curIdx * 2;
        rightIdx = curIdx * 2 + 1;
      }
    }

    return min;
  }
}
```

> 전체 코드 : [https://codesandbox.io/s/data-structure-by-javascript-gv68jw?file=/src/priorityQueue.js](https://codesandbox.io/s/data-structure-by-javascript-gv68jw?file=/src/priorityQueue.js)
