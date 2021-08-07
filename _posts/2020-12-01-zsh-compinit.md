---
toc: true
toc_sticky: true
title: "터미널 zsh compinit 에러 해결하기"
excerpt: "compinit 에러를 해결해보자."

categories:
  - Linux
tags:
  - Linux
  - zsh
--- 

## :( Trouble

iTerm에서 설치했던 oh-my-zsh 테마를 업데이트했는데 아래와 같은 에러가 떴다.

zsh compinit: insecure directories, run compaudit for list.  
Ignore insecure directories and continue [y] or abort compinit [n]?
{: .notice--warning}
물론 여기서 y를 입력해서 무시할 수 있으나, 터미널을 실행할 때마다 매번 나오는 에러 메시지를 보기 싫었기 때문에 이참에 해결하기로 했다. 😎

우선 y를 입력하면 아래와 같은 에러 메시지가 주르륵 뜬다.

```bash
[oh-my-zsh] Insecure completion-dependent directories detected:
drwxrwxr-x  4 seonghyeon  admin  128 12  1 03:05 /usr/local/share/zsh
drwxrwxr-x  7 seonghyeon  admin  224 11  5 00:15 /usr/local/share/zsh/site-functions
...
[oh-my-zsh] To fix your permissions you can do so by disabling
[oh-my-zsh] the write permission of "group" and "others" and making sure that the
[oh-my-zsh] owner of these directories is either root or your current user.
[oh-my-zsh] The following command may help:
[oh-my-zsh]     compaudit | xargs chmod g-w,o-w

[oh-my-zsh] If the above didnt help or you want to skip the verification of
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.
```

영어를 잘 하지 못해도 대충 읽어보면 해결 방법은 아래의 두 가지이다.


## :) Solution
### 방법 1. 그룹과 기타 사용자의 쓰기 권한 제거하기

[oh-my-zsh] the write permission of "group" and "others" and making sure that the  
[oh-my-zsh] owner of these directories is either root or your current user.  
[oh-my-zsh] The following command may help:  
[oh-my-zsh] compaudit | xargs chmod g-w,o-w  
<br> : `group` 과 `others` 의 쓰기 권한을 확인해라 그리고 `compaudit | xargs chmod g-w,o-w` 명령어가 도움이 될 것이다.
{: .notice--success}

**chmod g-w,o-w**

```bash
compaudit | xargs chmod g-w,o-w
```
이 명령어를 실행하면 문제가 생기는 디렉터리의 group 과 others 사용자들의 쓰기 권한이 제거된다.
> xargs 명령어는 앞에서 compaudit 명령어로 얻어진 결과물을 그대로 매개변수로 받아 chmod 명령어의 적용 대상으로 사용하는 명령어이다.



### 방법 2. 에러 메시지 무시하기

[oh-my-zsh] If the above didnt help or you want to skip the verification of  
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to  
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.  
<br> : 이 에러 메시지를 무시하려면 zshrc 파일이 sourced 되기 전에 `ZSH_DISABLE_COMPFIX` 변수를  `true` 로 설정해라.
{: .notice--success}

**ZSH_DISABLE_COMPFIX = "true"**

```bash
$vi ~/.zshrc
############################
plugins=(git)

# oh-my-zsh setting
ZSH_DISABLE_COMPFIX="true"	# 이 위치에 적어준다.

source $ZSH/oh-my-zsh.sh
```  
위와 같이 vi로 zshrc 파일을 열어서 source 바로 윗 부분에 해당 변수와 값을 입력해주면 터미널을 실행할 때 자동으로 에러 메시지를 무시하게 된다.
  
## :0 Think

물론 명령어를 그대로 실행하면 해결된다. 하지만 무작정 명령어를 실행하기 전에 문제가 되었던 `/usr/local/share/zsh` 디렉터리를 확인해 봤다.

```bash
ls -l /usr/local/share/
drwxr-xr-x  103 developerh  admin  3296 11  8 13:25 aaa
drwxr-xr-x    7 developerh  admin   224  8 27 14:46 bbb
..
drwxrwxr-x    4 developerh  admin   128 12  1 03:05 zsh # 얘만 그룹 허가권이 rwx 이다.
```

잘 보면 다른 디렉터리들의 퍼미션은 755(`drwxr-xr-x`)인데 zsh 디렉터리와 하위에  site-functions 디렉터리만 퍼미션이 775(`drwxrwxr-x`)이다. 즉, 그룹 사용자들에게도 쓰기 권한이 주어진 것이다. 이 문제는 `방법 1` 에서 그룹 사용자의 쓰기 권한을 제거하면서 해결할 수 있다. 그런데 생각해보면 어차피 내 계정은 admin 그룹에 속해있는 관리자 계정이다. admin 그룹에 속한 계정들의 쓰기 권한을 굳이 제거할 필요는 못 느꼈기 때문에 `방법 2` **를 따라서 에러 메시지를 무시하는 방향으로 해결하였다.**

> MacOS의 admin 계정
>
> : macOS에서 생성되는 모든 사용자는 staff 그룹에 소속된다. 그 중에서도 처음 생성되는 사용자는 "관리자"로 생성되는데, 이 사용자는 staff 그룹과 admin 그룹에 함께 소속된다.



## Reference

> * [Oh My Zsh 설치 & Plugins](https://goax.tistory.com/4) 
>
> * [macOS 시스템 환경설정 - 사용자 및 그룹](https://kimsungjin.tistory.com/402) 