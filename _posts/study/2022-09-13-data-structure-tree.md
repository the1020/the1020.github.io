---
title: 비선형구조, 트리
author: 예은
date: 2022-09-13 19:20:00 +0900
categories: [study]
tags: [자료구조]
subtitle: [비선형구조, 트리]
---

# 🚧⚒️🔧🔨🚜🦺👷🏻 정리중...

트리는 리스트와 달리 **비선형구조**이다. 리스트가 기차 구조라면 트리는 나무에 비유할 수 있다. 나뭇가지는 여러 가지들로 뻗어나가는 것처럼 트리는 하나의 요소에 연결된 요소들이 2개 이상일 수 있다. 리스트는 기차의 각 열차처럼 앞뒤로 최대 하나의 요소와 연결할 수 있다.

트리의 중요한 특징은 노드 간의 관계는 **부모-자식 관계만 성립**될 수 있다. 즉, 하나의 노드가 여러 개의 자식 노드를 가질 수 있지만 **형제 노드와는 연결될 수 없다**.(형제 노드 간 연결된 구조는 그래프이다.) 또한 **자식 노드는 부모 노드를 가리킬 수 없다**. 트리에서 모든 노드는 루트에서 멀어지는 방식(아래쪽으로 향한다.)으로 연결된다. 그리고 이 **루트는 트리의 시작점이며 반드시 하나**여야 한다.

## 실생활에서 사용되는 트리의 개념

### HTML DOM

HTML은 여러 요소들로 구성되어 있고, 한 요소 안에 다른 요소가 중첩되어 있다. 브라우저는 이 HTML 파일을 읽어 DOM을 생성한다. HTML은 단순 텍스트로 구성되지만 브라우저는 이를 객체 모델로 변환한다. DOM의 구조는 최상단 요소인 `<html>`을 시작으로 자식 요소들이 중첩되어 있다. 개발자 도구의 `Elements` 탭에서 DOM과 가장 유사한 형태의 트리 구조를 확인할 수 있다. DOM 자체가 아닌 유사한 형태라고 한 이유는 개발자 도구가 제공하는 트리 형태의 DOM 객체에는 가상 요소가 포함되기 때문이다. 이와 관련된 내용은 아래 포스트를 참고하자.

> 🐝 참고
>
> [(번역) DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)

### 컴퓨터 폴더 구조

컴퓨터의 파일 저장 구조를 보면 폴더 안에 또 다른 폴더 또는 파일이 존재하고, 폴더 내 자식 폴더를 생성할 수 있다.

### JSON

JSON의 데이터 구성은 한 개의 노드 값을 가지고 있는 `parent Object`에 자식 노드값들로 구성되어 있다.