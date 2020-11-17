---
title: "add부터 push까지!"
excerpt: "add부터 push까지의 플로우를 친절하게 설명해보았다."

categories:
  - Git
tags:
  - Git
  - add
  - commit
  - push
---  
최근에 친구가 commit이 무엇이냐고 물었다. 나는 git을 흘러가는 대로 사용하고 있었을 뿐이라 정확한 플로우를 설명하지 못했다. 때문에 자극을 받은 나는 이 포스팅 위에 누군가에게 설명하듯 정리하면서 다시 한번 개념들을 환기시켜보았다. 내 지식을 기반으로 했기 때문에 찰떡같은 설명은 아닐 수도 있다.


우선 git은 연결리스트 처럼 세 가지 공간이 유기적으로 연결되어있다.

![2020-10-23-flowChart](/assets/images/git/2020-10-23-flowChart.png)

### add

내가 실제 작업공간인 `workspace` 안에서 작업을 하면 그 변경 사항들이 `working-area` 에 담기게 된다. 여기서 `add` 를 하게 되면 변경 사항들이 `staging-area` 로 이동하게 된다. `staging-area` 로 이동된 파일들은 저장될 준비 완료!상태가 되며 이 상태를 `staged` 라고 한다. 

### commit

이제 `commit`을 하면 준비된 파일들이 `local-repository` 에 저장될 것이다. 여기서 실제로 파일이 저장되는 것은 아니고 **오직 변경 내역만 한 장의 스냅샷처럼 저장된다. 이 스냅샷이 비로소 하나의 의미있는 version이라고 할 수 있다.** 

### push

이 과정을 반복하다가 모든 작업이 끝나면 `push` 명령어를 통해서 github와 같은 `remote-repository` 의 원격으로 연결된 브랜치(ex master, branch1, branch2 .. )에 올라가게 된다.

여기 까지가 로컬과 서버가 버전들을 관리하는 기본적인 work-flow이다.

추가적으로, `local-repository` 는 `worksapce` 안에 존재하는 `.git` 파일 자체이다. 따라서 `commit` 된 이력들은 이 파일 안에 고스란히 저장되어 있다. 이를 확인하는 명령어는 `git reflog`이다.

### Summary

길게 풀어서 설명했지만 요약하자면 간단하다.

1. 작업을한다.
2. 작업한 내용을 `add`해서 `staged` 상태로 만든다.
3. `commit` 명령어로 `.git` 파일에 변경 내역을 저장한다.
4. `push` 하여 원격 저장소로 업로드한다.
5. `git reflog` 명령어를 통해 commit된 이력들을 열람할 수 있다.



## origin?

`remote-repository` 에 push를 할 때 아래와 같이 입력한다. 여기서 `origin` 은 무슨 의미일까?

```bash
git push -u origin master
```

origin이란 원격 저장소의 이름인데, 처음에 원격 저장소를 추가할때 `git remote add` 명령어를 사용한다. 이때 `git remote <url>`형식으로 원격저장소를 추가하거나 `git clone`을 통해서 원격 저장소를 복사한다면 자동으로 origin라는 이름으로 원격 저장소가 등록된다. 따라서 origin이 아닌 별도의 이름을 지정해주고 싶다면 `git remote add <이름> <url>`와 같은 형태로 추가해주면 된다.

또한, `master` 는 생성되어있는 branch 중 가장 중심이 되는 `default branch` 이다.