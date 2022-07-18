---
title: Git의 restore, revert, reset의 차이점
author: 예은
date: 2022-07-12 23:48:00 +0900
categories: [study]
tags: [Git]
subtitle: [restore, revert, reset의 차이점]
---

## restore, revert, reset의 차이점

여러번 반복해도 이 3개가 너무 헷갈린다.

- `restore` : 수정사항을 되돌린다.

  ```
  //👇 HEAD가 참조하는 파일명에 해당하는 파일의 내용으로 되돌린다. 워킹트리에 존재하는 수정사항을 로컬 저장소의 내용으로 되돌린다.
  git checkout HEAD [파일명]
  git restore [파일명]

  //👇 HEAD의 특정 커밋 또는 1단계 전 커밋의 내용으로 HEAD가 이동한다.
  git checkout [커밋ID]
  git checkout HEAD~1

  //👇 해당 커밋의 파일 내용으로 복구한다. HEAD가 이동하는 것이 아니라 파일 내용만 복원한다.
  git restore --source [HEAD~1 또는 커밋ID] [파일명]

  //👇 스테이징된 파일을 되돌린다. (git add 취소)
  git restore --staged [파일명]
  ```

- `reset` : **커밋만 삭제**한다. 현재 워킹트리에서 작업하는 내용은 그대로 유지된다.

  ```
  //👇 커밋ID 이후의 커밋 기록은 모두 삭제된다.
  git reset [커밋ID]

  //👇 커밋ID에 해당하는 내용으로 모두 되돌아간다. 커밋ID 이후의 커밋 기록은 물론 수정사항이 모두 삭제된다.
  git reset --hard [커밋ID]
  ```

- `revert` : 커밋을 엎어쓴다. 커밋 자체를 삭제하지 않고 새로운 커밋으로 이전 커밋을 복원시킨다.

  ```
    git revert [커밋ID]
  ```

> 🐝 참고
>
> [Git & Github 실무 활용 완벽 가이드](https://www.udemy.com/course/best-git-github/)
