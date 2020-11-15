---
toc: true
toc_sticky: true
title: "commit 자유롭게 이동하기"
excerpt: "checkout 과 상대참조를 이용하여 자유롭게 과거 시점으로 돌아가보고 reflog 명령어로 손실된 커밋의 내역을 확인해보자."

categories:
  - Git
tags:
  - Git
  - checkout
  - reflog
---

# Relative Refs - 상대참조(^)

* 명령어를 통해 현 위치를 기준으로 시점을 이동할 때 사용한다.
* 매번 `git log` 명령으로 해시를 확인할 필요가 없다.

* 전 커밋으로 이동 :  `^`
* 지정한 수 만큼 전 커밋으로 이동 : `~<num>`


## commit 이동하기

### 특정 커밋으로 이동

* git log 명령어로 commit hash값을 확인하고
* 이동할 hash 값의 맨 앞에서 6자리 고유번호만 기억하자.

```bash
$ git log   # commit 로그 확인
commit b30af8e59913c9a2ff4b7d08531e6c1c4a609ab3 (HEAD -> master, origin/master, origin/HEAD)
Author: heoseongh <shheo0807@gmail.com>
Date:   Thu Oct 8 17:47:19 2020 +0900

    branchTest
    
commit ae797ffa4ac14340a1100729d0c824da1fb6da16   # 앞 6자리만 기억
Author: SeongHyeonHeo <64491331+heoseongh@users.noreply.github.com>
Date:   Thu Oct 8 16:50:16 2020 +0900

    Initial commit
```

```bash
$ git checkout ae797f   # ae797f 커밋으로 이동
Note: switching to 'ae797f'.
...
HEAD의 현재 위치는 ae797ff Initial commit
```

### 이전 커밋으로 이동

```bash
$ git checkout master^        # master 브랜치의 이전 커밋으로 이동
$ git checkout master^^       # master 브랜치의 전전 커밋으로 이동
$ git checkout user^          # user 브랜치의 이전 커밋으로 이동
```

```bash
$ git checkout HEAD^          # 현재 브랜치의 이전 커밋으로 이동
$ git checkout HEAD^^         # 현재 브랜치의 전전 커밋으로 이동
$ git checkout HEAD~3         # 현재 브랜치의 3전 커밋으로 이동
```

### 최근 커밋으로 복귀

```bash
$ git checkout master         # mater 브랜치의 최근 커밋으로 복귀
$ git checkout user           # user 브랜치의 최근 커밋으로 복귀
```



## 분리된 HEAD?

커밋을 이전 시점(`ae797ff`)으로 이동한 뒤에 파일을 수정하고 commit 한 뒤 push 했더니 에러가 났다. HEAD가 다음 시점 `ae797ff` 에서 분리되었다고 한다.  무슨 일이 일어난 것일까? 아래의`log`를 살펴보자.

```bash
# 새로운 시점(003bfe)이 생성되었다!
commit 003bfebbd65f84dab39b1b2d51c221fe0a1da399 (HEAD)
Author: heoseongh <shheo0807@gmail.com>
Date:   Thu Oct 8 18:11:46 2020 +0900

    branchTest


commit ae797ffa4ac14340a1100729d0c824da1fb6da16
Author: SeongHyeonHeo <64491331+heoseongh@users.noreply.github.com>
Date:   Thu Oct 8 16:50:16 2020 +0900

    Initial commit
```

기존의 HEAD가 위치했던 커밋(`b30af8`)이 아닌 새로운 커밋(`003bfe`) 내역이 출력되는 것을 볼 수 있다. 또한 HEAD가 가리키는 브랜치가 없는 것을 볼 수 있다.

처음에는 '아. 이전 커밋(`ae797ff`) 으로부터 새로운 브랜치가 만들어졌구나!' 라고 착각했었다. 그러나 push까지 진행해보니 아래와 같은 에러 메시지를 받았다.
```bash
fatal: 현재 어떤 브랜치 위에도 있지 않습니다.
지금 현재 (HEAD 분리) 상태까지의 커밋 내역을 푸시하려면
다음과 같이 하십시오.

git push origin HEAD:<name-of-remote-branch>
```
 결국 master 브랜치로부터 **새로운 브랜치가 만들어진 것이 아니라 새로운 시점만 생성된 것**이므로 **1)특정 브랜치에 push**를 하거나 **2)해당 커밋으로부터 새로운 브랜치를 만들어줘야 한다**는 것을 알게 되었다.

1) 특정 브랜치에 push 하기
```bash
$ git push <origin> <master>  # origin 저장소의 master 브랜치에 push
```

2) 해당 커밋으로부터 새로운 브랜치 생성
```bash
$ git branch <new> 003bfeb    # new 브랜치를 만들면서 HEAD를 003bfeb 커밋에 연결 
```

만약 이러한 연결작업 없이 최근 커밋으로 돌아온다면, 해당 커밋은 어떠한 브랜치도 갖고 있지 않기 때문에 찾을 방법이 없을 것이고 그대로 손실하게 된다. 그래도 당황하지 말자! `git reflog` 로 지금까지 HEAD의 참조 기록을 확인하면 된다.

```bash
$ git reflog
...
b30af8e (HEAD -> main, origin/main, origin/HEAD) HEAD@{8}: checkout: moving from 003bfebbd65f84dab39b1b2d51c221fe0a1da399 to main
003bfeb HEAD@{9}: commit: branchTest	# <-- 손실된 커밋을 찾았다.
ae797ff HEAD@{10}: checkout: moving from main to main^
...
```

> `git reflog` 를 이용하면 reset `--hard` 옵션으로 완전히 제거되었던 커밋도 되돌릴 수 있다.
>
> ```bash
> $ git reset --hard 003bfeb
> ```
> 이렇게 하면 commit : `003bfeb` 의 시점으로 완벽하게 되돌아온다. 결국, 거의 모든 상황에서 커밋 내역은 복구가 가능하다.



## 그렇구나!

브랜치에서 이전 커밋으로 이동해서 새로운 작업을 하고 commit을 하면 변경 사항이 기존의 브랜치에 적용되는 것이 아니라 그 시점에서 새로운(`HEAD와는 분리된`) 커밋만이 생성된다. 이 커밋은 어떠한 브랜치에도 포함되지 않는 혼자 붕 떠있는 커밋이다. 이 분리된 커밋을 `git branch <브랜치명> <커밋해시>` 명령어를 통해 브랜치화 해주면 비로소 유의미한 작업이 된다.