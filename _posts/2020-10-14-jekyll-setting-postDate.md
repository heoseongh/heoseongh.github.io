---
title: "jekyll 블로그 포스팅 날짜로 바꾸기"
excerpt: "포스팅 목록에 표시되는 소요시간을 날짜로 바꿔보자!"

categories:
  - GitBlog
tags:
  - Git
  - GitBlog
  - jekyll
---  
## 서론
한 블로거의 포스팅을 따라 열심히 블로그를 만들었다. 그러다 문득 포스트 제목 아래 표시되는 '포스팅을 읽는데 예상되는 소요시간(read-time)'이 필요 없다고 느껴졌다. 날짜로 바꾸려고 아무리 구글링을 해봐도 최신 버전의 방법이 나오지 않았기 때문에 결국 내가 직접 삽질을 했다.

우선 어떻게 바꿀 것인지 결과물을 보고 시작하자.

![](https://raw.githubusercontent.com/heoseongh/heoseongh.github.io/main/assets/images/jekyll/2020-10-14-setting-postDate.png)


구글링을 해보면 포스팅된 글들의 목록을 보여주는 부분(`_includes/archive-single.html`)과 글을 보여주는 부분(`_includes/single.html`) 2군데를 수정하면 된다고 하는데 아무리 건드려도 안된다..

```html
...
  </h2>
    { % include page__meta.html type=include.type % }
      { % if post.excerpt % }
        <p class="archive__item-excerpt" itemprop="description">
          { { post.excerpt | markdownify | strip_html | truncate: 160 } }
        </p>
      { % endif % }
  </article>
</div>
```

* { % include page_meta.html % } : page_meta.html 파일을 끼워넣는다.

> 위와 같은 Liquid 언어는 Ruby 기반의 언어로 jekyll에서 사용하는 언어이다.
>
> 참고: [JekyllDocs](https://jekyllrb-ko.github.io/docs/)

이제 page_meta.html 파일을 살펴보자. 크게 조건문 3개가 보인다.

```ruby
{ % if document.read_time or document.show_date % }
{ % if document.read_time and document.show_date % }
{ % if document.read_time % }
```

1. `read_time` 이거나 `show_date` 이면?
2. `read_time` 이고 `show_date` 이면?
3. `read_time` 이면?

이것을 보면 대충 이전에 `_config.yml`에서 `read_time`을 `true`로 설정한 기억이 떠오를 것이다.

 `_config.yml`로 돌아가서 포스트 부분에 `read_time`을 `false`로 바꿔주고 `show_date: true`를 추가하자. 이쁘게 주석도 달아놓자.

```yaml
defaults:
  # _posts
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: false   # 읽는데 걸리는 소요시간 표시
      show_date: true    # 포스팅 날짜 표시
      comments: true
      share: true
      related: true
```

이렇게 설정 파일을 건드렸기 때문에 새로 고침으로는 적용이 안되고 서버를 다시 실행시켜줘야 한다.

```bash
$ bundle exec jekyll serve	# 서버 재실행
```

이렇게 하면 소요시간을 감추고 포스팅 날짜만 표시되게 설정할 수 있다.

## 마치며..

이전 버전까지는 `includes/archive-single.html` 파일에 목록에 대한 설정 코드가 다이렉트로 들어갔지만 최근 버전부터는 시간, 날짜 관련 메타 정보들은 따로 관리하게끔 `includes/page__meta.html` 파일에 관련 코드들을 묶어논 듯하다. 아마 이전에는 _config.yml에서 read_time과 show_date(과거 post.date)를 따로 설정할 수가 없었나 보다. 

