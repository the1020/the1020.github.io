---
title: 타입스크립트의 심화 - 함수
author: 예은
date: 2022-09-23 18:24:00 +0900
categories: [study]
tags: [타입스크립트]
subtitle:
  [
    Typescript 기초부터 실전형 프로젝트까지 with React + NodeJS 강의를 보면서 정리하는 글,
  ]
---

> ✍️ [Typescript :기초부터 실전형 프로젝트까지 with React + NodeJS](https://www.udemy.com/course/best-typescript-21/) 강의를 보면서 정리하는 글

## 함수 반환 타입 명시하기

함수의 반환 타입을 지정하는 방법은 아래처럼 함수 우측에 콜론과 반환할 타입을 명시해주면 된다.

```javascript
// 👇 매개변수로 두 개의 숫자를 받아 숫자를 반환하는 함수
function add(num1: number, num2: number): number {
  return num1 + num2;
}
```

아무것도 반환하지 않는 함수는 `void`로 명시해주면 된다.

```javascript
function add(num1: number, num2: number): void {
  console.log(num1 + num2);
}

// ✅ 반환 타입을 undefined로 지정할 수도 있는데 이 경우에는 return 문은 존재해야 한다.
// 따라서 대다수의 경우에는 void로 반환 값이 없음을 표현한다.
function add(num1: number, num2: number): void {
  console.log(num1 + num2);
  return; //또는 return undefined;
```

함수 타입을 변수의 타입으로 명시할 수도 있다. `Function`으로 타입을 명시하면 함수만 할당할 수 있다. 하지만 매개변수와 반환 타입 같은 세세한 설정은 할 수 없어 어떤 함수든 할당될 수 있다.

```javascript
function add(num1: number, num2: number): number {
  return num1 + num2;
}

function addString(str1: string, str2: string): string {
  return str1 + str2;
}

let combineValues: Function;
combineValues = add;
combineValues(4, 5); // 9

// ⚠️ 문자열을 받아 문자열을 반환하는 함수이지만 함수 타입이기만 하면 할당되고 호출할 수 있기 때문에 타입 체크가 되지 않는다.
// 컴파일과 런타임 모두 에러가 발생하지 않는다.
combineValues = addString;
console.log(combineValues(4, 5)); // 9
```

매개변수와 반환 타입까지 정확한 타입을 명시할 수 있다.

```javascript
// 👇 이 변수는 두 개의 숫자를 매개변수로 받으며 숫자를 반환하는 함수만 할당할 수 있다.
let combineValues: (a: number, b: number) => number;
combineValues = add;
console.log(combineValues(4, 5));

// ⚠️ ERROR!
// '(str1: string, str2: string) => string' 형식은 '(a: number, b: number) => number' 형식에 할당할 수 없습니다.
// 'str1' 및 'a' 매개 변수의 형식이 호환되지 않습니다.
// 'number' 형식은 'string' 형식에 할당할 수 없습니다.
combineValues = addString;
```

> 🐝 참고
>
> [타입스크립트에서 함수 문법](https://ui.toast.com/weekly-pick/ko_20210521)

## unknown과 any

`any` 타입을 쓰면 타입스크립트는 원하는대로 작성하게 해준다. `unknown`은 `any`보다 더 제한적이다.

```javascript
let userInput: unknown;
let userName: string;

// 👇 문자열을 할당해도 unknown 타입이므로 string 타입으로 인식되지 않는다.
userInput = "yeeun";

// ⚠️ ERROR!
// 'unknown' 형식은 'string' 형식에 할당할 수 없습니다.
userName = userInput;
```

```javascript
let userInput: any;
let userName: string;

// 👇 any는 유연한 타입이므로 타입 확인을 수행하지 않는다.
userInput = "yeeun";
userName = userInput;
```

## never

리턴하지 않는 함수(`while(true){}`)) 또는 항상 예외를 던지는 함수일 때 발생한다.
리턴하지 않는다는 것은 아무것도 반환하지 않는 `void`와는 완전히 다른 값이므로 혼동하지 않도록 해야한다.

함수 선언식은 함수를 기본으로 `void`로 추론하는 반면 함수 표현식은 `never`로 추론한다.

- 아무런 타입도 명시하지 않은 함수 선언식
  ![use-function-declaration-without-never](/assets/img/post/TIL/20220923/use-function-declaration.png)

- 아무런 타입도 명시하지 않은 함수 표현식
  ![use-function-expressions](/assets/img/post/TIL/20220923/use-function-expressions.png)

- `never`를 명시한 함수 선언식
  ![use-function-declaration-with-never](/assets/img/post/TIL/20220923/use-function-declaration-with-never.png)

> 🐝 참고
>
> [Never 타입](https://radlohead.gitbook.io/typescript-deep-dive/type-system/never)
>
> 👇 아직 이해하기 어려운 글... 나중에 다시 읽어봐야겠다.
>
> [타입스크립트의 Never 타입 완벽 가이드](https://ui.toast.com/weekly-pick/ko_20220323)
