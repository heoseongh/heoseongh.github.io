---
title: "Git 과거로 돌아가자 - 상대참조(Relative Refs)"
---
# # 상대참조(^) (Relative Refs)

* 명령어를 통해 현 위치를 기준으로 시점을 이동할 때 사용한다.
* 매번 `git log` 명령으로 해시를 확인할 필요가 없다.

* 전 커밋으로 이동 :  `^`
* 지정한 수 만큼 전 커밋으로 이동 : `~<num>`

## commit 이동하기

#### 특정 커밋으로 이동

* git log 명령어로 commit hash값을 확인하고
* 이동할 hash 값의 맨 앞에서 6자리 고유번호만 기억하자.

```bash
$ git log		# commit 로그 확인
commit b30af8e59913c9a2ff4b7d08531e6c1c4a609ab3 (HEAD -> main, origin/main, origin/HEAD)
Author: heoseongh <shheo0807@gmail.com>
Date:   Thu Oct 8 17:47:19 2020 +0900

    branchTest
    
commit ae797ffa4ac14340a1100729d0c824da1fb6da16	# 앞 6자리만 기억
Author: SeongHyeonHeo <64491331+heoseongh@users.noreply.github.com>
Date:   Thu Oct 8 16:50:16 2020 +0900

    Initial commit
```

```bash
$ git checkout ae797f		# 특정 커밋으로 이동
Note: switching to 'ae797f'.
...
HEAD의 현재 위치는 ae797ff Initial commit
```

#### 이전 커밋으로 이동

```bash
$ git checkout master^		# master 브랜치의 이전 커밋으로 이동
$ git checkout master^^		# master 브랜치의 전전 커밋으로 이동
$ git checkout user^		# user 브랜치의 이전 커밋으로 이동
```

```bash
$ git checkout HEAD^			# 현재 브랜치의 이전 커밋으로 이동
$ git checkout HEAD^^			# 현재 브랜치의 전전 커밋으로 이동
$ git checkout HEAD~3			# 현재 브랜치의 3전 커밋으로 이동
```

#### 최근 커밋으로 복귀

```bash
$ git checkout master			# mater 브랜치의 최근 커밋으로 복귀
$ git checkout user				# user 브랜치의 최근 커밋으로 복귀
```



## # 이동을 해서 어디다 쓸까?

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

`log`를 통해서 기존의 커밋(`b30af8`) 내역이 아닌 새로운 커밋(`003bfe`) 내역이 출력되는 것을 볼 수 있다.

새로운 커밋(`003bfe`)이 생성되었기 때문에 '아. 이전 커밋(`ae797ff`) 으로부터 새로운 브랜치가 만들어졌구나!' 라고 생각했지만 더 진행해보니 **아직 브랜치는 만들어지지 않았고, 새로운 시점만 생성된 것**이므로 아래와 같이 **기존의 브랜치에 연결해주는 작업이 필요**하다.

```bash
$ git branch <새-브랜치-이름> 003bfeb
```

연결하지 못하고 최근 커밋으로 돌아와버렸다고 당황하지 않아도 된다. `git reflog` 로 지금까지 HEAD의 참조 기록을 확인하면 된다.

```bash
$ git reflog
...
b30af8e (HEAD -> main, origin/main, origin/HEAD) HEAD@{8}: checkout: moving from 003bfebbd65f84dab39b1b2d51c221fe0a1da399 to main
003bfeb HEAD@{9}: commit: branchTest	# <-- 커밋했던 내역을 찾았다.
ae797ff HEAD@{10}: checkout: moving from main to main^
...
```

> `git reflog` 를 이용하면 `hard-reset`도 되돌릴 수 있다.
>
> ```bash
> $ git reset --hard 003bfeb
> ```
>
> 결국, 거의 모든 상황에서 커밋은 복구가 가능하다.



## # 이해한것들

이전 커밋으로 이동해서 파일을 수정하고 `commit`을 하면 그 시점에서 새로운(`HEAD와는 분리된`) 커밋이 생성되며 이 커밋을 명령어를 통해 `master branch`에 연결해주면 `new branch`가 만들어지는 것이다. 

이론으로만 알고 있었는데 실제로 다뤄보니  정말 유용할 것 같다는 생각이 들었다. `new branch` 에서 수정한 것을 `commit`하고 `push`하기 위해서는 기존의 브랜치와 합병해주는 `merge` 작업을 수행해야 했다. 

프로젝트시 팀장(`master branch`)과 여러 기능들을 구현하는 팀원(`other branches`)들을 두고 팀원들이 각각의 브랜치에서 기능을 구현한 뒤에 commit을 하고 그 기능들을 merge해서 최종적으로 팀장이 수정한 뒤에 push하는 것이라고 이해했다. 이것이 협업의 시작이로구나!