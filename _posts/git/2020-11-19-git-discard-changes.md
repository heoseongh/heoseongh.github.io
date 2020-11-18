---
title: "git 수정된 변경사항 버리는 방법"
excerpt: "원하는 파일의 변경사항만 푸쉬하고 나머지는 버리자!"

categories:
  - Git
tags:
  - Git
  - checkout
---  

# :( Truble

feature 브랜치에서 기능 개발을 마친 후 커밋을 하려고 한다. 그런데 개발한 기능과는 상관 없는 파일까지도 변경사항에 추적이 된다. 나도 모르게 상관 없는 파일까지 건드렸나보다.
{: .notice--danger}

# :0 Think

원하는 파일만 선택해서 커밋을 하고 나머지 변경 사항은 버리는 방법이 없을까? VScode에서 살펴보니 Discard Changes라는 버튼이 있었다. 그러면 터미널에서도 가능하겠구나!
{: .notice--success}

# :) Solution

```bash
$ git checkout .
```
끝이다.

만약 이미 커밋을 해버렸다면 1)커밋 전으로 돌리고 2)원하는 파일만 푸쉬 3)나머지 변경사항 제거
```bash
$ git reset HEAD^
$ # 원하는 파일 푸시 #
$ git checkout .
```


## Reference
> <http://hochulshin.com/git-revert-changes/>{: target="_blank"}
> : 위 블로그에서는 상황별로 git을 되돌릴 수 있는 솔루션을 많이 챙겨갈 수 있으니 참고해보자!