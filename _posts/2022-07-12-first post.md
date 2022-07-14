---
title: 드디어 블로그 만들기 완성🏌️‍♀️
author: 예은
date: 2022-07-12 15:29:00 +09:00
categories: [jekyll]
tags: [jekyll]
---

## ⛳️ 블로그 생성

예전에 github 블로그를 만들었다가 복잡하고 원하는대로 되지 않아서 포기했었는데 오늘 갑자기 다시 만들고 싶어서 Jekyll를 이용해서 블로그를 만들었다.

이번에도 처음부터 오류나고 안넘어가서 하지말까하다가 계속 쉬운 블로그 찾아다니며 어째저째 만들었다.

[왕초보를 위한 Github 블로그 만들기](https://zeddios.tistory.com/1222)가 진짜 도움이 많이 되었고
[Jekyll 공식 가이드](https://jekyllrb-ko.github.io/docs/)보면서 ruby 환경설정을 했다.

![Jekyll](/assets/img/post/first_post/Jekyll.png)

~~몇 초는 무슨.. 나는 세시간 걸렸다.~~

세시간 아님
깃헙에 배포하는거까지 하루종일 걸렸음

로컬에서는 잘나오는데 깃헙으로 배포할 때 계속 오류가 났다. `Actions` 탭에서 확인해보면 jekyll-theme-chirpy을 계속 못찾는다고 했다.
소스를 zip으로 받아서 설치해서 설정을 했었는데 소스를 fork 받아서 다시 만들었다.

왜 초기 세팅은 쉽게 되는 일이 없을까...

오랜만에 머리 쥐어짰다. 맥북 아니었으면 주먹으로 화면 한 대 쳤을거다.

## ⛳️ 블로그 초기 설정

### 🪄 Skipping: \_posts/2022-07-12-first post.md has a future date

포맷에 맞춰서 글을 추가하고 브라우저에서 확인했는데 글이 안나왔다. 터미널에 이런 문구가 떴고
![skippost](/assets/img/post/first_post/skippost.png)

찾아보니 한국 시간대에 맞춰서 작성해줘야 한다고 한다.
`_config.yml`에서 `timezone: Asia/Seoul`으로 설정했으니 그에 맞게 UTC+09:00로 설정해주어야 하는거 같다.

![setTimezone](/assets/img/post/first_post/setTimezone.png)

> 참고 [jekyll 에서 drafts 혹은 post가 인식되지 않을 때.](https://hurderella.tistory.com/128)

> 📡 `_config.yml`에서 `future:true`로 설정하면 현재 시점 이후에 작성된 글도 보여준다.

### 🪄 폰트 변경하기

영문으로 볼 때는 예뻤는데 한글은 내 스타일이 아니라서 폰트를 변경했다.
`/sass/addon/commons.scss`와 `/sass/addon/module.scss`에 원하는 폰트를 import하고 설정하니 바로 적용되었다.

지마켓에서 만든 `Gmarket Sans`를 적용해뒀는데 조만간 바꿀 거 같다.

### 🪄 메인 화면

블로그의 메인 화면이 `pin: true`을 설정한 글만 나타나게 설정되어있다. 이걸 바꿀지 말지.. 고민이다..
고정시킬 글들이 많을까 싶고 무슨 기준으로 고정을 해두나 싶고 글이 없다면 메인 화면이 비어보이고..
