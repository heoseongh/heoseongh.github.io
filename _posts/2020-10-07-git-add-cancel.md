---
toc: true
toc_sticky: true
title: "add / commit 취소하기"
excerpt: "실수로 원하지 않는 파일까지 Staging Area에 넣은 경우, 되돌리고 싶을 때가 있을 것이다."

categories:
  - Git
tags:
  - Git
  - add
  - commit
---
## add 취소하기

실수로 git add . 명령어를 사용해서 원하지 않는 파일까지 Staging Area에 넣은 경우, 되돌리고 싶을 때가 있을 것이다.

방법은 두 가지가 있다.

1. git restore
2. git reset

### add와 commit을 취소하는 원리는

1. master브랜치를 가리키고 있는 HEAD포인터를 이전 스냅샷으로 이동시키면 `commit`이 취소되는 것과 같고
2. 그 스냅샷의 index까지 이동한다면 `add`가 취소되는 것이다.

### 1) git restore

```bash
# 모든 파일이 Staged 상태로 바뀐다.
$ git add .
# 파일들의 상태를 확인한다.
$ git status
On branch master
Changes to be committed:
(use "git restore --staged <file>..." to unstage)
```

`Changes to be committed:` 아래 출력되는 메시지를 해석해보면 답이 나온다.

`git restore --staged <file>` 명령어를 통해서 unstage 상태로 갈 수 있다고 한다.

```bash
$ git restore --staged <file>		# 특정 파일 add 취소
$ git restore --staged .				# 모든 파일 add 취소
```

### 2) git reset

* 뒤에 파일명이 없으면 add한 파일 전체를 취소한다.
* `reset` 명령어는 `commit`을 취소할때도 옵션을 달리하여 사용한다. 

```bash
$ git reset HEAD <file>					# 특정 파일 add 취소
$ git reset HEAD								# 모든 파일 add 취소
```



## commit 취소하기

너무 일찍 commit을 했거나 어떤 파일을 빼먹고 commit한 경우 완료된 commit을 취소하는 방법을 알아보자.

### $ git reset <옵션> <돌아가고 싶은 커밋>

- 지정한 커밋 이후의 모든 commit history가 삭제된다.
- Ctrl + Z를 누른 것과 같은 효과

* 동작 원리 (HEAD -> D -> C -> B -> A)

  1. HEAD 포인터를 원하는 시점으로 이동시킨다. (HEAD -> C)

  2. 이전 history는 지워진다. (HEAD -> C -> B -> A)

### Option

#### 1) hard : commit 취소, 워킹디렉터리에서도 제거됨

```bash
$ git reset --hard HEAD^
```

> git에는 실제 데이터를 삭제하는 방법이 별로 없지만 `--hard` 옵션은 워킹 디렉터리의 원본  파일까지 돌려놓으므로 조심하게 사용해야 한다. 이 옵션을 사용해서 삭제된 데이터는 복구할 수 없게된다.

#### 2) soft : commit만 취소(staged 상태)

```bash
$ git reset --soft HEAD^
```

이 상태에서 아래와 같이 커밋 메시지만 바꾸어 push 한다면 `git commit --amend -m "message"` 와 동일한 효과를 낼 수 있다.

```bash
$ git commit -m "message"
$ git push --force
```

#### 3) mixed : commit 취소, add 취소(index 또는 stage 초기화)

```bash
$ git reset HEAD^ # default옵션이기 때문에 적지 않아도 된다.
```

