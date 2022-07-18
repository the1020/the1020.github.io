---
title: React의 컴포넌트간의 상태 관리
author: 예은
date: 2022-07-14 17:23:00 +0900
categories: [study]
tags: [React]
---

## 상태 관리하기

- `useState` 사용하기

```javascript
const [userList, setUserList] = useState("");

const addUser = (newUser) => {
  setUserList([...userList, newUser]);
};
```

- 2개 이상의 컴포넌트간의 이벤트와 데이터 전달
  - 하위 컴포넌트에서 상위 컴포넌트로 데이터 보내는 방법
  - 여러 컴포넌트에서 동일한 데이터를 사용한다면 그 컴포넌트들의 부모 컴포넌트에서 데이터의 상태를 관리하고 자식에게 전달해주면 된다.

> **연습 프로젝트**
>
> [https://codesandbox.io/s/zen-stallman-0vbzj0](https://codesandbox.io/s/zen-stallman-0vbzj0)

> 🐝 참고
>
> [React 완벽 가이드 with Redux, Next.js, TypeScript](https://www.udemy.com/course/best-react/)
