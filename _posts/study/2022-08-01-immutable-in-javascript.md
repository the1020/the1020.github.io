---
title: 참조 타입의 불변성 알아보기
author: 예은
date: 2022-08-01 18:50:00 +0900
categories: [study]
tags: [REACT, JAVASCRIPT, 불변성, 상태관리]
---

### 불변성(Immutable)

🔖 불변성이란 상태를 변경하지 않는 것을 의미한다.

🔖 불변성을 지키면 `1. 무분별한 상태의 변경을 막을 수 있고` `2.상태의 변경을 추적하기가 쉽다`.

🔖 자바스크립트에서 원시 타입은 불변성을 가지지만 참조 타입은 그렇지 않다. 참조 타입의 객체의 경우 메모리 힙 영역에 저장이 되어 내부 프로퍼티를 변경해도 같은 참조를 갖고 있다. 따라서 객체의 특정 프로퍼티만 변경하는 작업을 수행 후 다른 객체로 인식되지 않는다.

🔖 `Object.assign()` 메서드를 이용하거나 ES6의 `스프레드 연산자`를 이용하여 이전 참조와 다른 참조로 변경할 수 있다. 각각의 메모리 공간에 할당되어 다른 객체로 인식된다. 하지만 두 가지 방법 모두 **얕은 복사**를 수행하기 때문에 내부의 프로퍼티가 참조인 경우 참조를 그대로 복사하여 완전히 다른 참조가 되지는 못한다.

### 리액트에서의 불변성과 렌더링

🔖 리액트는 props나 state가 변경되었을 때 컴포넌트를 리렌더링하는데 변경된 사실은 불변성을 이용해서 감지한다. 객체의 참조를 복사한다는 점을 이용해 단순히 참조만 비교하는 얕은 비교를 이용해서 변경이 일어났는지 확인한다. 그렇기 때문에 [직접 state를 수정해서는 안된다.](https://ko.reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)

### 불변성 유지를 도와주는 라이브러리 (immer)

자바스크립트에서 불변성을 유지하려면 `Object.assign()` 메서드 또는 ES6의 `스프레드 연산자`를 이용해야 하는데 이마저도 얕은 복사를 수행하므로 객체의 프로퍼티가 참조 타입이면 재귀를 통해 복사를 해야하는 번거로움이 생긴다.

`immer`는 이를 대신하여 불변 객체를 관리해주는 자바스크립트 라이브러리다.

### React + Immer

리액트의 `useState`와 `useReducer` 내부에 저장된 상태 값들은 불변성을 지닌다. 따라서 직접적으로 상태 값을 변경하지 않고 setState를 이용하며 변경하고자 하는 상태 값은 새로운 객체를 생성한 뒤 업데이트를 요청한다.

```javascript
const changeState = (newItem) => {
    //1️⃣ 새로운 객체로 갱신한다.
    setState(newItem);

    //2️⃣ updateState라는 프로퍼티만 갱신한다.
    setState((prevState) => { ...prevState, updateState : updatedValue});
}
```

immer의 produce 함수를 useState와 useReducer에서 사용할 수 있으며 새로운 객체를 생성하여 전달하지 않고 파라미터로 받은 원래의 상태 값을 변경하기만 하면 된다.

### useReducer + Immer

어제 리액트 강의에서 만들었던 [Practice Project](https://codesandbox.io/s/building-a-food-order-app-dodgfg?file=/src/store/CartProvider.js)에서 장바구니 컴포넌트에서 `useReducer`로 상태 값을 관리하고 있다. 이 부분을 immer를 사용한 코드로 변경해보기로 했다.

#### 기존 장바구니 컴포넌트의 상태 관리

![cart-component-state](/assets/img/post/TIL/20220801/cart-component-state.png)

장바구니의 아이템 목록은 위와 같은 상태 값으로 구성되어 있다. 목록은 배열로 관리하고 각 요소들은 객체로 구성되어 있다.

장바구니에 새 아이템이 추가하면 기존 아이템 목록(`items`)에 동일한 아이템이 추가되어 있는지 확인하고 이미 추가되어 있다면 그 객체의 수량(`amount`) 프로퍼티 값만 증가시켜 업데이트한다. 기존 코드에서 amount 상태 값 갱신과 관련이 없는 구문들은 생략하였다.

![cart-reducer](/assets/img/post/TIL/20220801/cart-reduce.gif)

```javascript
//⚠️ 리듀서 함수는 호출되는 컴포넌트 외부에서 선언해도 되므로 CartProvider 외부에서 선언하였음
const reducerFn = (state, action) => {
  if (action.type === "ADD") {
    const existedItem = state.items[existedItemIndex];
    const updateItem = {
      ...existedItem,
      amount: existedItem.amount + action.payload.amount,
    };

    const updateItems = [...state.items];
    updateItems[existedItemIndex] = updateItem;

    return { items: updateItems, totalAmount: updatedTotalAmount };
  }
};

const CartProvider = (props) => {
  const [cartState, dispatchAction] = useReducer(reducerFn, defaultState);

  const addItemCartToHandler = (newItem) => {
    dispatchAction({ type: "ADD", payload: newItem });
  };
};
```

장바구니에 물품이 추가해서 이벤트가 발생하면 `addItemCartToHandler`이 호출되면서 리듀서 함수(`reducerFn`)가 실행된다.

`items`라는 상태는 참조 타입 내에 참조 타입을 가지고 있는 구조다.(객체를 요소로 갖는 배열)

그렇기 때문에 `items` 객체를 스프레드 연산자를 이용해 새 객체를 생성하더라도 내부 참조는 동일한 값을 가리킨다.

```javascript
const copyItems = [...state.items];
console.log(state.items[0] === copyItems[0]); // true
```

여기서 새 객체를 반환하기 위해서는 아래의 순서대로 작업이 필요하다.

1. 업데이트 할 요소를 찾아 `amount`값을 변경한 객체를 생성한다. (Line 4 ~ 8)
   - 배열의 인덱스로 업데이트할 배열의 요소를 찾은 후 스프레드 연산자로 새 객체를 생성하여 `updateItem`에 할당한다.
2. 그 후에 `items`를 스프레드 연산자로 새 객체를 생성한다. (Line 10 ~ 11)
   - 이전 상태 값과 다른 참조 값을 반환해야 하므로 스프레드 연산자를 이용하여 `updateItems`라는 변수에 새로운 객체를 할당한다. 그리고 1번에서 작업한 `updateItem` 요소를 업데이트해준다.

이처럼 새 객체를 반환하기 위해 스프레드 연산자를 사용한다. 이 부분을 immer로 대체해보자.

#### immer의 produce 활용하기

사용 방법은 [Immer 공식 문서](https://immerjs.github.io/immer/example-setstate#usereducer--immer)를 참고하였다.

```javascript
const reducerFn = produce((draft, action) => {
  if (action.type === "ADD") {
    const existedItem = draft.items[existedItemIndex];
    existedItem.amount += action.payload.amount;
  }
});

const CartProvider = (props) => {
  const [cartState, dispatchAction] = useReducer(reducerFn, defaultState);
  /* ... */
};
```

immer의 produce 함수의 첫 번째 파라미터로 리듀서 함수를 전달한다.

이 때 리듀서 함수는 원래 상태 값인 `draft`와 dispatch 함수가 보낸 `action` 값을 받을 수 있다. items에서 업데이트 할 항목을 찾은 후 재할당할 필요없이 바로 값을 갱신해주면 된다.

### useImmerReducer

`useImmer` 패키지의 `useImmerReducer`는 더 간단하게 immer를 사용할 수 있도록 해준다.

useReducer 대신 useImmerReducer를 사용하면 produce 함수를 생략하고 기존 함수를 전달해줘도 동일하게 동작한다.

```javascript
const reducerFn = (draft, action) => {
  if (action.type === "ADD") {
    const existedItem = draft.items[existedItemIndex];
    existedItem.amount += action.payload.amount;
  }
};

const CartProvider = (props) => {
  const [cartState, dispatchAction] = useImmerReducer(reducerFn, defaultState);
  /* ... */
};
```

### 마무리

자바스크립트에서 불변성 유지를 쉽게 도와주는 라이브러리는 immer 이 외에도 [lodash의 cloneDeep](https://lodash.com/docs/4.17.15#cloneDeep), [immutable-js](https://immutable-js.com/) 등이 있다.

매번 스프레드 연산자로 얕은 복사를 한 뒤 새 객체를 반환해주거나 깊은 복사 함수를 구글링해서 사용했는데 이런 라이브러리를 사용하면 수월하게 작업할 수 있을 거 같다.
위에서 작업한 것처럼 코드도 굉장히 간단해져서 가독성도 훨씬 좋다.

> 🐝 참고
>
> - [불변 객체와 immer](https://ui.toast.com/weekly-pick/ko_20220217)
>
> - [변하지 않는 상태를 유지하는 방법, 불변성(Immutable)](https://evan-moon.github.io/2020/01/05/what-is-immutable/)
>
> - [immer 공식 문서](https://immerjs.github.io/immer/)
