---
title: "jekyll serve 에러 [error: Could not find 'eventmachine']"
excerpt: ""

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
  - TrubleShooting
---  

## :( Truble

macOS 카탈리나 -> 빅서로 업데이트 한 직후, 블로그를 모니터링하기 위해 터미널에서 `jekyll serve` 명령어를 실행했더니 다음과 같은 에러가 떴다.

```bash
Could not find 'eventmachine' (>= 0.12.9) among 94 total gem(s) (Gem::MissingSpecError)
Checked in 'GEM_PATH=/Users/seonghyeon/.gem/ruby/2.6.0:/Library/Ruby/Gems/2.6.0:/System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/gems/2.6.0', execute `gem env` for more information
```

구글링을 해보니 bundle 명령어로 gem 버전을 업데이트해주면 된다고 하여 `bundle exec jekyll serve` 명령어를 실행해봤지만 또 다른 에러만 낳을 뿐이었다.



## :0 Think

내 가정은 두가지였다.

1. 빅서로 업데이트 되면서 보안이 강화되어 업데이트가 방해받는 것인가?
2. 빅서부터는 루비가 자동으로 포함되지 않는 것인가? = 루비를 따로 설치해야 하나?

1번 가정을 베이스로 2번 가정에 접근해보니 `rbenv` 를 통해서 루비 버전 관리를 하는 것이 낫겠다고 판단했다. 그래서 `brew install rbenv ruby-build` 명령어로 필요한 재료를 설치하려는데 여기서도 에러가 발생했다.

```bash
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

xcrun 에러는 macOS 업데이트시 매번 발생하는 xcode clt 이슈이다. 이 에러를 해결하면서 bundle exec jekyll serve 명령어가 실행이 되었고, gem 버전이 업그레이드 되면서 jekyll serve 에러 또한 해결이 되었다. **결국 xcode command line tools 이슈를 해결하는 것이 해답이었다.** 


## :) Solution

xcrun 에러를 해결하는 방법은 [MacOS Big Sur로 업데이트 이후 에러](/macos/error-xcrun-when_updating_macos) 포스팅을 따라해보자.

