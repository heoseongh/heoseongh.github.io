---
title: "jekyll 포스트 상단에 업데이트 날짜 표시하기"
excerpt: "게시글 제목 아래에 날짜를 표시해보자!"

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
---  
오늘은 제목 밑에 포스팅 날짜를 설정해보기 위한 삽질을 해보았다. 이제는 구조가 대충 예상이 되어서 이 부분을 건드리면 되겠구나! 하는 경지에 이르렀다.

## 결과물

![2020-10-16-pageDate](https://raw.githubusercontent.com/heoseongh/heoseongh.github.io/main/assets/images/jekyll/2020-10-16-setting-pageDate.png)

시간이 없다면 [commit f72906](https://github.com/heoseongh/heoseongh.github.io/commit/f7290634095fb738e49c9c80f2de5c8d6a0ba5d3#diff-547ab8d2983a8826bba71ca77904355bbd8556fafa5ed274deaa975eb453763f)을 참고해서 빠르게 수정하자.

## 따라해보기

크롬의 개발자 모드로 열심히 구조를 파악해 본 결과.

* single.html
* page__date.html

위 두개만 건드리면 된다. 정확하게 말하면 `page__date.html`파일을 `single.html`의 header 아래 부분에 포함시켜주면 된다.

여기서 `single.html`은 단위 페이지를 구성해서 뿌려주는 역할을 하고, `page__date.html`은 날짜 정보만을 구성하는 역할을 한다.

```html
<header>
{ % if page.title % }<h1 id="page-title" class="page__title" itemprop="headline">{{ page.title | markdownify | remove: "<p>" | remove: "</p>" }}</h1>
{ % endif % }
{ % include page__date.html % }	<!-- 이 한 줄을 추가시켜 주자. -->
</header>
```