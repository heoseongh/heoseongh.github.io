# git add / commit 취소하기

## # 선수지식

> 깃의 세 가지 상태

- Committed  : 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미한다.
- Modified      : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것을 말한다.
- Staged          : 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태를 의미한다.

> 깃의 세 가지 단계

* Working Tree : 특정 버전을 Checkout 한 것이다.

* Staging Area   : add 명령어를 사용했을 때 Stage Fixes된 상태를 저장하는 파일. 즉, 곧 커밋할 파일에 대한 정보를 저장한다. 기술적인 용어로는 `index` 라고 한다.
* .git directory (Repository)  : Git이 프로젝트의 `메타데이터와` `객체 DB`를 저장하는 곳. git의 핵심. 다른 저장소를 `Clone`할때 Git 디렉토리가 만들어진다.

> 깃이 하는 일

1. 워킹 트리에서 파일을 수정한다.
2. Staging Area에 있는 파일을 Stage해서 커밋할 스냅샷을 만든다. 모든 파일을 추가할 수도 있고 선택하여 추가할 수도 있다.
3. Staging Area에 있는 파일들을 커밋해서 Git 디렉토리에 영구적인 스냅샷으로 저장한다.

> 세 개의 트리

* Git = 세 가지 트리를 관리하는 시스템

* 트리 = 파일의 묶음 (index가 트리는 아니지만 이해를 쉽게 하기  위해 트리라고 하자.)

| 트리              | 역할                                              |
| ----------------- | ------------------------------------------------- |
| HEAD              | 마지막 커밋 스냅샷, 현재 브랜치를 가리키는 포인터 |
| Index             | 다음에 커밋할 스냅샷                              |
| Working Directory | 샌드박스                                          |

#### add와 commit을 취소하는 원리는

1. master브랜치를 가리키고 있는 HEAD포인터를 이전 스냅샷으로 이동시키면 `commit`이 취소되는 것과 같고
2. 그 스냅샷의 index까지 이동한다면 `add`가 취소되는 것이다.

## # add 취소하기

실수로 git add . 명령어를 사용해서 원하지 않는 파일까지 Staging Area에 넣은 경우, 되돌리고 싶을 때가 있을 것이다.

방법은 두 가지가 있다.

1. git restore
2. git reset

### $ git restore

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

### $ git reset

* 뒤에 파일명이 없으면 add한 파일 전체를 취소한다.
* `reset` 명령어는 `commit`을 취소할때도 옵션을 달리하여 사용한다. 

```bash
$ git reset HEAD <file>					# 특정 파일 add 취소
$ git reset HEAD								# 모든 파일 add 취소
```



## # commit 취소하기

너무 일찍 commit을 했거나 어떤 파일을 빼먹고 commit한 경우 완료된 commit을 취소하는 방법을 알아보자.

### $ git reset

* 동작 원리

  1. HEAD 브랜치를 이동시킨다.

  2. 현재 브랜치가 가리키는 commit을 바꾼다.

```bash
$ git reset --soft HEAD^	# commit 취소
$ git reset --mixed HEAD^	# commit, add 취소
$ git reset HEAD^					# --mixed 옵션과 동일
$ git reset HEAD~2				# 마지막 2개의 commit 취소
$ git reset --hard HEAD^	# commit 취소 / 해당 파일들은 워킹디렉터리에서 삭제
```

git에는 실제 데이터를 삭제하는 방법이 별로 없지만 `--hard` 옵션은 워킹 디렉터리의 원본  파일까지 돌려놓으므로 조심하게 사용해야 한다. 이 옵션을 사용해서 삭제된 데이터는 복구할 수 없게된다.

##### + 만약, 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌리고 싶으면, 아래의 명령어를 사용한다.

* 단, 이 명령을 사용하면 원격 저장소에 있는 마지막 commit 이후의 워킹 디렉터리와 add했던 파일들이 모두 사라지므로 주의해야 한다.

```bash
$ git reset --hard HEAD		# 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌린다.
```

```bash
$ git reset --hard HEAD^
$ git reset -
```

