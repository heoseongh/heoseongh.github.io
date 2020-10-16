---
toc: true
toc_sticky: true
title: "jekyll 에러에 대처하는 법"
excerpt: "커밋 이후 갑자기 로컬 서버가 실행되지 않을 때. 대처법."

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
  - reset
  - solution
---  
## 상황

한참 이것 저것 재미있게 건드리고 나서 git에 push하고 나니 갑자기 로컬 서버가 돌아가지 않았다.

```bash
$ bundle exec jekyll serve
Doing `require 'backports'` is deprecated and will not load any backport in the next major release.
Require just the needed backports instead, or 'backports/latest'.
Configuration file: /Users/seonghyeon/mygit/heoseongh.github.io/_config.yml
            Source: /Users/seonghyeon/mygit/heoseongh.github.io
       Destination: /Users/seonghyeon/mygit/heoseongh.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
------------------------------------------------
      Jekyll 4.1.1   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
------------------------------------------------ 
```

직전까지 분명 돌려보고 push했는데 이게 무슨일이지? 포트가 실행중인가? 검색해봐도 열려있는 포트는 없었다. 머리를 쥐어짜내서 어떤 설정을 건드렸는지 생각해 보았지만 생각이 안난다. 이것저것 만져보다가 로컬 디렉터리가 완전 꼬여버렸다. 로컬을 원상복구 할 필요가 생겼다. 그렇다. git은 이럴때 써먹으라고 있는 것이 아닌가!

## git reset을 활용해보자

저번 [git add / commit 취소하기](https://heoseongh.github.io/git/git-add-cancle/) 포스팅을 하면서 확실하게 익혔던 `git reset --hard HEAD^` 를 통해서 로컬을 통채로 push 이전으로 되돌렸다. 로컬의 파일들이 커밋 이전의 시점으로 전부 되돌아왔다. 다시 서버를 돌려보니 잘 돌아간다! 최근 커밋 전까지는 이상이 없었음을 알 수 있다. 

무엇이 문제였을까? 궁금해서 다시 미래로 가봤다. `git reflog`로 reset전 `hash id`를 찾아내고 `git reset --hard <hash id>`명령으로 복귀했다. 그리고 나서 서버를 돌려봤는데 잘 돌아간다.. 뭐지.. (ㅎ-ㅎ) 곧바로 강제 푸쉬(`git push -f`)해서 리모트 저장소에 overrite했다. 다행히 건드렸던 설정 그대로 유지할 수 있게 되었다. 하마터면 전부 다시 바꾸어야 할 뻔 했다.



## 마치며

아는 만큼 친숙해진 덕분일까? 처음에는 깃을 무서워했는데 이제는 무섭지 않다. github 블로그 만들어보면서 위기의 상황을 마주할 때 마다 몇 가지 명령어로 해결되는 것이 너무 짜릿하다. 앞으로도 순탄하지 않았으면(??..) 좋겠다.

hard-reset, 위험한 만큼 확실하다.

```bash
$ git reset --hard
```

