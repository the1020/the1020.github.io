---
title: 타입스크립트의 기초 - 타입
author: 예은
date: 2022-09-22 19:08:00 +0900
categories: [study]
tags: [타입스크립트]
subtitle:
  [
    Typescript 기초부터 실전형 프로젝트까지 with React + NodeJS 강의를 보면서 정리하는 글,
  ]
---

> ✍️ [Typescript :기초부터 실전형 프로젝트까지 with React + NodeJS](https://www.udemy.com/course/best-typescript-21/) 강의를 보면서 정리하는 글

## JavaScript와 TypeScript 차이

> **타입스크립트 타입은 컴파일 중에 확인되고 자바스크립트 타입은 런타임 중에 확인된다.**

자바스크립트는 `동적 타입`으로 하나의 변수에 문자열도 넣었다가 숫자를 넣어도 문제가 발생하지 않는다. 타입을 체크하거나(`typeof`) 관련 에러는 런타임 시 확인 가능하다.

반면, 타입스크립트는 `정적 타입`이다. 개발 도중에 타입을 체크하고 관련 에러를 미리 방지할 수 있다. 자바스크립트 엔진에는 타입스트립트의 이런 타입 체크 기능이 내장되어 있지 않기 때문에 **타입스크립트를 사용하면 개발 도중에만 지원을 받을 수 있다.**

- 🔖 타입스크립트는 새로 만들어진 언어가 아닌 자바스크립트를 사용해 새로운 기능과 장점을 추가한 언어이다. 자바스크립트를 더 쉽고 강력하게 해준다.

- 🔖 타입스크립트는 브라우저와 같은 자바스크립트 환경에서 실행할 수 없다. 따라서 타입스크립트로 코드를 작성 후 타입스크립트의 문법들을 자바스크립트 해결책으로 컴파일한다.

  - 🔖 타입을 위한 구문으로 자바스크립트로 컴파일할 때 코드를 평가하고 잠재적 에러를 찾는데 사용되지만 컴파일 후 얻게 되는 자바스크립트 코드에서는 제거된다. 자바스크립트의 문법이 아니기 때문이다.

- 🔖 타입스크립트는 우리가 의도를 더 명확히 하고 코드를 다시 확인하도록 요구한다.

## 변수에 값을 할당할 때는 타입을 지정하지 않아도 된다.

`let number1 = 10;`과 같이 변수 선언과 동시에 값을 할당하는 경우 타입 추론(type inference)라는 내장 기능을 활용하여 number라는 변수는 number 타입이라는 것을 알 수 있다. 따라서 타입을 지정하지 않아도 된다.

다만 `let number1;`처럼 변수 선언문만 쓴다면 추후 값을 할당 시 타입 체크를 위해 변수명 우측에 타입을 지정해주는 것이 좋다. (`let number1: number;`)

![string is not assignable to type 'number'](/assets/img/post/TIL/20220922/string-is-not-assignable-to-type-number.png)

또한 이미 특정 타입의 값이 할당된 변수에 다른 타입의 값을 재할당하는 경우도 타입스크립트는 에러를 발생시킨다.

![string is not assignable to type 'number'](/assets/img/post/TIL/20220922/string-is-not-assignable-to-type-number2.png)

타입스크립트는 이처럼 타입을 잘못 사용하고 있는지 확인하고 에러를 통해 개발 단계에서 바로 잡을 수 있도록 알려주는 작업을 수행한다.

## 타입스크립트의 튜플과 Enum

### 튜플

튜플은 배열의 서브타입이다. 배열과 달리 튜플은 **선언 시 길이와 각 인덱스의 타입을 명시해야 한다.** 튜플과 배열 모두 대괄호를 사용하기 때문에 튜플의 타입을 명시하지 않는 경우 타입스크립트는 배열로 타입을 추론하게 된다.

튜플을 사용하면 배열의 요소의 타입과 개수를 고정할 수 있지만 `push()` 메서드로 요소를 삽입하여 정해진 길이보다 길어지더라도 오류가 발생하지 않는다.

```javascript
let test: [number, string] = [1, "second"];

test.push(10);
console.log(test); //(3) [1, 'secode', 10]

// ⚠️ ERROR!
// 길이가 '2'인 튜플 형식 '[number, string]'의 인덱스 '3'에 요소가 없습니다.
test[3] = 100;

// ⚠️ ERROR!
// '[number, string, number]' 형식은 '[number, string]' 형식에 할당할 수 없습니다. 소스에 3개 요소가 있지만, 대상에서 2개만 허용합니다.
test = [10, "repeat", 30];
```

### Enum(열거형)

열거형은 해당 타입으로 사용할 수 있는 값을 열거한다.

`enum` 키워드로 enum을 생성하고 이름을 명시해준다. 중괄호 안에 사용할 키 값을 열거해주면 된다. 자동으로 열거형의 각 멤버에 숫자를 추론해서 할당하지만, 값을 명시적으로 설정할 수도 있다.

```javascript
// 👇 멤버에 값을 명시하지 않은 경우 순서대로 0, 1, 2의 값을 가지게 된다.
enum Fruit {
    APPLE,
    ORANGE,
    BANANA
}

console.log(Fruit[0]);      // APPLE
console.log(Fruit.ORANGE);  // 1

// 👇 각 멤버에 값을 명시해줄 수 있다. 숫자와 문자열을 혼합할 수 있다.
enum Fruit {
    APPLE = 1,
    ORANGE = 2,
    BANANA = "BANANA"
}

console.log(Fruit.ORANGE);  // 2

// 👇 모든 멤버에 값을 명시해줘야 하는 것은 아니다. 빠진 값은 타입스크립트가 추론한다.
enum Fruit {
    APPLE = 3,
    ORANGE = 100,
    BANANA
}
console.log(Fruit.BANANA) /// 101

```
