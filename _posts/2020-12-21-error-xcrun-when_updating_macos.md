---
toc: true
toc_sticky: true
title: "MacOS Big Sur로 업데이트 이후 에러 [xcrun: error: invalid active developer path]"
excerpt: ""

categories:
  - MacOS
tags:
  - TrubleShooting
--- 

## :( Truble

<span style="background-color:yellow; color:red;">ERROR: xcrun: error: invalid active developer path</span>

macOS를 `Big Sur` 로 업그레이드하고 나서 깃을 사용하려고 했더니 위와 같은 에러가 뜬다. 검색해보니 예전부터 업데이트를 할 때마다 `Xcode Command Line Tools` 관련해서 이슈가 발생했다고 한다. 다음 업데이트에도 또 이러한 이슈가 발생할 수 있으니 메모를 해둬야겠다!

> 맥에서 컴파일 툴(`gcc`,`make`)이나 버전 관리 툴(`svn`,`git`)을 사용하려면 기본적으로 `Homebrew`나  `Command Line Tools` 를 설치해야 한다.
>
> `Xcode Command Line Tools` 는 Xcode 라는 맥 전용 IDE를 설치하면 함께 설치되는데 용량이 크기 때문에(대략 5GB) Xcode를 사용하지 않으려면 터미널에서 명령어(`xcode-select --install`)를 통해 따로 설치가 가능하다.



## :) Solution

### Xcode Command Line Tools 설치

1. 터미널에서 명령어 실행

	```bash
	$ xcode-select --install
	```

2. 설치된 gcc 확인

   ```bash
   $ gcc --version
   ```

3. 출력된 내용으로부터 path 확인

   * /Library/Developer/CommandLineTools/usr/bin

   ```bash
   Configured with: --prefix=/Library/Developer/CommandLineTools/usr --with-gxx-include-dir=/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/4.2.1
   Apple clang version 12.0.0 (clang-1200.0.32.28)
   Target: x86_64-apple-darwin20.2.0
   Thread model: posix
   InstalledDir: /Library/Developer/CommandLineTools/usr/bin
   ```

   

