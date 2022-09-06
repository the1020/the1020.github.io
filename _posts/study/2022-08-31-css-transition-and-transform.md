---
title: transition과 transform 사용하기
author: 예은
date: 2022-08-31 15:23:00 +0900
categories: [study]
tags: [CSS]
---

# 🚧⚒️🔧🔨🚜🦺👷🏻 정리중...

# CSS Transitions and Transforms for Beginners

`transforms`는 요소의 모양을 이동하거나 변경하고, `transitions`는 요소를 부드럽고 점진적으로 현재와는 다른 상태로 바꿔준다.

## transitions

`transitions`없이 요소의 모양이 변경하면 하나의 상태에서 다른 상태로 불쑥 변경된다. `transitions`을 사용함으로써 더 유연하게, 자연스럽게 변화하도록 제어할 수 있다.
즉, `transitions` 속성을 사용하면 요소의 형태가 변하는 과정을 확인할 수 있다.
`transforms`뿐만 아니라 요소의 형태가 변하는 스타일 속성에서 다양하게 사용할 수 있다.(width가 변하거나 color가 변하는 경우)

width의 기본 값인 auto일 때에는 애니메이션이 적용되지 않으므로 숫자 값으로 설정해줘야 한다.

`transitions`를 설정하기 위해서 사용해야 하는 필수 속성은 2가지가 있따.

1. `transition-property`
2. `transition-duration`

transition의 속성을 각각 정의할 수도 있지만 깔끔한 코드를 위해 축약해서 사용하는 것을 권장한다.

```css
div {
  transition: [property] [duration] [timing-function] [delay];
}
```

### transition-property (_required_)

애니메이션을 적용시킬 속성을 지정한다.

```css
div {
  /* 👇 transition을 background-color에만 적용하고 싶다면 */
  transition-property: background-color;

  /* 👇 transition을 모든 속성에 적용하고 싶다면 */
  transition-property: all;
}
```

### transition-duration (_required_)

애니메이션을 지속시킬 시간을 지정한다. 모든 속성에 적용할 시간을 명시하거나, 각 속성이 각자 다른 주기를 갖도록 시간을 명시할 수도 있다.

```css
.div {
  /* 👇 transition을 300밀리 초만큼 지속 */
  transition-duration: 300ms;

  /* 👇 transition을 1초만큼 지속 */
  transition: all 1s;

  /* 👇 transition을 2초만큼 지속 */
  transition: all 2s;
}
```

### transition-timing (optional)

애니메이션의 속도를 지정한다. 기본 값은 ease로 점차 빨라지다가 끝날 쯤에 속도를 늦춘다. 다른 옵션으로 `linear`, `ease`, `ease-in`, `ease-out`, `ease-in-out`가 있다.

> 🐝 참고
>
> [timing의 옵션별 예시](https://codepen.io/rachelcope/pen/gbxzmo)

```css
div {
  transition-timing-function: ease-in-out;
}

div {
  transition: all 3s ease-in-out;
}
```

### transition-delay (optional)

애니메이션의 시작을 지연시킬 수 있다. 기본적으로 `transition`은 트리거되는 즉시 시작되는데 이를 트리거된 시점 후에 동작시키고 싶다면 delay 속성을 지정하면 된다.

```css
div {
  transition: all 3s 1s;
}
```

## transforms

지금까지 요소의 변화 과정을 자연스럽고 부드럽게 보여줄 수 있는 방법에 대해 알아보았다. 이제 하나의 상태에서 다른 상태로 요소를 변화시킬 수 있는 `transform`에 대해 알아보자.

`transform`을 사용하여 요소를 회전, 이동, 기울이기 및 크기 조정할 수 있다. 마우스를 올리거나 클릭하는 것과 같이 요소가 상태를 변경할 때 `transform`이 트리거된다.

### scale

요소의 크기를 늘리거나 줄일 수 있다. 예를 들어 2라는 값은 요소의 기본 크기에서 2배를 키워주고 0.5는 절반으로 크기가 줄어든다.

가로, 세로에 대한 파라미터를 각각 설정해서 크기를 조정할 수 있다. 예를 들면, `transform: scaleX(2)`와 같이 쓴다.

아니면 `scale()`로 가로와 세로를 동일한 비율로 키울 수도 있다. `transform: scale(2)`로 지정하면 가로, 세로가 2배씩 커지고 `scale()`에 각각 파라미터를 전달해줘도 된다 : `transform: scale(2, 4)`

### rotate

### skew

> 🐝 참고
