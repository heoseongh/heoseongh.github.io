---
toc: true
toc_sticky: true
title: "jekyll 텍스트 블럭 하이라이트 스타일 커스텀하기"
excerpt: "하이라이트 색상을 입맛대로 바꿔보자!"

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
  - Custom
---

# 하이라이트 스타일 커스텀하기

블로그를 만들고 어느 정도 포스팅을 하고 나니 세부적인 설정에 관심이 가기 시작했다. minimal-mistakes-master 테마는 텍스트 블럭에 하이라이트가 들어갔는지 의문이 들 정도로 전경색이 연하다. 또한 글자 색상도 더 튀는 색으로 바꾸고 싶었다. 때문에 직접 바꿔보기로 했다.



## 하이라이트 글자색 바꾸기

`_sass/minimal-mistakes/_base.scss` 파일의 171번째 쯔음부터 code 하이라이트를 설정할 수 있는 속성 값들을 볼 수 있다. td > code {...} 안에 아무 곳에나 color 속성을 추가해주면 된다.

```scss
td > code {
  ...
  background: $code-background-color;
  
  border-radius: $border-radius;
  color : #C72542; // 이 부분을 추가해준다.
 ...
}
```



## 하이라이트 배경색 바꾸기

위와 같은 위치에서 `background` 속성값에 원하는 배경색을 넣어주면 되는데, `code-background-color` 라는 변수에 따로 색상 정보가 관리된다. 변수를 지우고 `background: #F9F2F4`와 같은 형식으로 넣어주자. 

```scss
td > code {
  ...
  background: #F9F2F4;	// 속성 값을 원하는 색상으로 바꿔준다.
  border-radius: $border-radius;
  color : #C72542;
 ...
}
```

>  `command` + `shift` + `F` 를 눌러서 전역검색으로 `code-background-color` 변수가 어디에 선언되어있는지 찾아보자. 검색을 해보면 대충 알겠지만 `_dark`, `_aqua` 와 같은 테마들이 SCSS 파일로 존재한다. 이와 같이 다양한 테마에서 적용되는 같은 색상들을 재활용 하기 위해 변수를 통해서 관리하는 것이다. 
>
>  그런데, 이 변수를 추적해서 디폴트 색상을 바꾼다면, 나중에 다른 테마를 적용했을 때에도 그 색이 디폴트 색상으로 적용되어버린다. 예를 들면 다크모드에서 글씨의 색이 검정색으로 나오는 등 각 테마에서 의도되는 스타일이 나오지 않게 된다.  따라서 그냥 테마별로 커스텀하자.