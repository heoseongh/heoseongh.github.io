---
toc: true
toc_sticky: true
title: "jekyll 포스트 작성시 notice 스타일 적용하기"
excerpt: "notice 스타일을 적용하여 텍스트 블럭을 예쁘게 장식해보자"

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
---

# NOTICE TEXT BLOCKS

다른 블로그를 보다가 글 텍스트블럭에 예쁘게 색이 들어가 있는 것을 보았다. 나도 써보고 싶어서 크롬 개발자 모드로 요리조리 뜯어봤다. 그렇게 알아낸 Notice 스타일의 적용법을 공유한다.
> `minimal-mistakes` 테마를 기준으로 설명할 것이다. 다른 테마에서도 Notice 스타일이 있는지 알아보려면 전역검색(`command` + `shift` + `f`)으로 'notice'를 검색해보자.



## Notice 스타일 적용법

스타일을 적용하고 싶은 문단의 바로 아래에 아래의 코드를 넣어주면 된다.

```text
기본 Notice 입니다.
스타일을 적용하고 싶은 문단 바로 아래
코드를 추가해 주시면 됩니다.
{: .notice} <--- 요렇게!
```

기본 Notice 입니다.
스타일을 적용하고 싶은 문단 바로 아래
코드를 추가해 주시면 됩니다.
{: .notice}
 
**주요 Notice 입니다.**  
`{: .notice--primary}`를 추가해주세요.
{: .notice--primary}

**정보 Notice 입니다.**  
`{: .notice--info}`를 추가해주세요.
{: .notice--info}

**경고 Notice 입니다.**  
`{: .notice--warning}`를 추가해주세요.
{: .notice--warning}

**성공 Notice 입니다.**  
`{: .notice-success}`를 추가해주세요.
{: .notice--success}

**위험 Notice 입니다.**  
`{: .notice--danger}`를 추가해주세요.
{: .notice--danger}



## _notices.scss

notice 스타일 관련 코드들은 `_notices.scss` 파일을 참고하면 된다.

```scss
/* Default notice */
.notice {
  @include notice($light-gray);
}

/* Primary notice */
.notice--primary {
  @include notice($primary-color);
}

/* Info notice */
.notice--info {
  @include notice($info-color);
}

/* Warning notice */
.notice--warning {
  @include notice($warning-color);
}

/* Success notice */
.notice--success {
  @include notice($success-color);
}

/* Danger notice */
.notice--danger {
  @include notice($danger-color);
}
```