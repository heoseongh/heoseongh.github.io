---
title: "[git] .DS_Store 파일을 git에 포함되지 않도록 설정하기"
---
## # 문제점

`MacOS` 에서는 시스템이 폴더에 접근할 때 해당 폴더에 대한 메타데이터를 저장하는 파일인  `.DS_Store` 파일이 생성된다. 이 파일은 윈도우의 `thumb.db` 파일과 비슷한데, 이 파일을 분석해보면 해당 디렉토리의 크기, 아이콘의 위치, 폴더 배경에 대한 정보들을 얻을 수 있다. 이 파일은 github에 함께 공유되지 않도록 `gitignore` 설정을 해주는 것이 좋다.



## # 해결방안

만약, 이미 `.DS_Store`가 커밋되었다면 다음 명령어를 사용한다. `.DS_Store`를 찾아서 0번째 인자로 받고 이를 `$ git rm`하는 명령어이다.

```bash
find . -name .DS_Store -print0 | xargs -0 git rm --ignore-unmatch
```

명령어를 통해 모든 `repository`에 직접 들어가서 지운 뒤 `.gitignore_global`파일을 셋팅해주면 앞으로는 모든 `repository`에서 전역적으로`.DS_Store`파일은 걸러지게 된다. 

```bash
$ echo ".DS_Store" >> ~/.gitignore_global
$ echo "._.DS_Store" >> ~/.gitignore_global
$ echo "**/.DS_Store" >> ~/.gitignore_global
$ echo "**/._.DS_Store" >> ~/.gitignore_global
$ git config --global core.excludesfile ~/.gitignore_global
```



## # 새로 알게된 것

각각의 `repository`에 `.gitignore`를 두어 `commit`되지 않을 파일을 지정해 주는 방법 뿐만 아니라 최상위 디렉토리에 `.gitignore_global`이라는 설정파일을 두어 `전역적으로 commit되지 않게` 관리해줄 수 있다는 것을 처음 알게되었다.

