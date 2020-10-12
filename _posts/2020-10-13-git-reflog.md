---
title: "[git] reset --hard 복구하기"
---
`git reflog` : git에서 보관하는 이력들을 볼 수 있는 명령어

#### `git reflog` 를 이용하면 `hard-reset`도 되돌릴 수 있다.

1. `git reflog` 명령어로 삭제된 commit의 hash id를 확인한다.

```bash
$ git reflog
...
b30af8e (HEAD -> main, origin/main, origin/HEAD) HEAD@{8}: checkout: moving from 003bfebbd65f84dab39b1b2d51c221fe0a1da399 to main
003bfeb HEAD@{9}: commit: branchTest	# <-- 커밋했던 내역을 찾았다.
ae797ff HEAD@{10}: checkout: moving from main to main^
...
```
2. `git reset --hard <id>` 명령어로 복구!

```bash
$ git reset --hard 003bfeb # 혹은 HEAD@{9}
```

이제부터 과거로 돌아가버렸다면 그리고 history가 날아갔다면? 당황하지 말고 `git reflog` 가 있다는 사실을 기억하자! 