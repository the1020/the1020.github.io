---
title: 타입스크립트의 심화 - 컴파일러
author: 예은
date: 2022-10-27 18:43:00 +0900
categories: [study]
tags: [타입스크립트]
subtitle:
  [
    Typescript 기초부터 실전형 프로젝트까지 with React + NodeJS 강의를 보면서 정리하는 글,
  ]
---

> ✍️ [Typescript :기초부터 실전형 프로젝트까지 with React + NodeJS](https://www.udemy.com/course/best-typescript-21/) 강의를 보면서 정리하는 글

# 🚧⚒️🔧🔨🚜🦺👷🏻 정리중...

## 관찰 모드 (watch)

`--watch` 또는 `-w`를 사용하면 타입스크립트가 파일을 관찰하고 파일에 변경 사항이 있을 때마다 다시 컴파일한다. 관찰 모드는 종료하지 않는 ts 파일의 업데이트를 감지한다.

```
tsc app.ts --watch
tsc app.ts -w
```

## 다수의 파일을 컴파일해야 한다면

`tsc 파일명1, 파일명2, ...`와 같이 컴파일 할 파일들을 나열한다.

```
tsc app.ts extra.ts
```

**특정 파일을 지정하지 않고 프로젝트의 모든 타입스크립트 .ts 파일을 컴파일하기 위해서는**

`tsc --init`으로 해당 경로를 타입스크립트 프로젝트라고 초기화한다. 이 명령어를 수행하면 `tsconfig.json` 파일이 생성된다. 이 파일은 프로젝트를 컴파일하는 데 필요한 옵션을 지정한다.

`tsc`를 실행하면 해당 경로와 하위에 있는 ts파일들을 컴파일한다.

`tsc --init` 후 `tsc --watch(또는 -w)`로 모든 파일에 대해 관찰 모드를 실행할 수 있다.

## tsconfig.json의 여러 옵션들

- `compilerOptions`

  - 타입스크립트 코드가 컴파일되는 방식을 관리한다.

    - `target` : 컴파일 후 생성되는 자바스크립트의 버전
    - `lib` : 설정이 안 되어 있으면 기본 설정은 `target` 설정에 따라 달라진다.
      es6로 설정한 경우, es6에서 전역적으로 사용 가능한 모든 기능을 타입스크립트에서도 사용할 수 있다.

      > 🐝 lib 설정에 대한 자세한 설명은 아래 블로그 참고
      >
      > [[Typescript] tsconfig.json의 lib](https://norux.me/59)

- `include`

  - `tsc`를 실행하여 컴파일에 포함시킬 파일 또는 경로를 지정한다.

- `exclude`

  - `tsc`를 실행하여 컴파일 시 포함되어서는 안되는 파일 또는 경로를 지정한다.
  - `exclude` 속성에 디렉토리가 지정되지 있지 않다면 기본적으로 _node_modules_, _bower_components_, _jspm_packages_ 그리고 \<outDir\>를 제외합니다.
  - `include`에 지정한 파일이나 패턴을 제외시킬 수 있다. 주의할 점은 include에 지정하지 않은 파일은 적용되지 않는 점이다.

  ```json
  "exclude" : [
      "extra.ts",
      "node_modules", /* node_modules 디렉토리 내 모든 파일 무시 */
      "*.dev.ts" /* dev.ts가 포함된 모든 파일 무시 */
  ]
  ```

- `files`

  - 컴파일 할 개별 파일들을 지정한다.

  ```json
  "files": [
      "app.ts",
      "types.ts"
  ]
  ```

> 🐝 참고
>
> [tsconfig.json](https://typescript-kr.github.io/pages/tsconfig.json.html)
>
> [{ tsconfig.json } 제대로 알고 사용하기](https://velog.io/@sooran/tsconfig.json-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
