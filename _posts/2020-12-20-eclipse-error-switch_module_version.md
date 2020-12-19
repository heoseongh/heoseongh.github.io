---
toc: true
toc_sticky: true
title: "Tomcat ERROR : The currently selected server type does not support remote hosts"
excerpt: ""

categories:
  - JSP
tags:
  - JSP
  - Eclipse
  - Setting
  - Tomcat
  - TrubleShooting
--- 

## :( Truble
저번 포스팅 [**<이클립스 서버 런타임 환경 설정하기: 아파치 톰캣>**](/jsp/tip-eclipse-setting-RuntimeServerEnvironment)에서 마지막에 예고했던 대로 다음의 에러를 해결해 보자.

Run on Server 로 JSP 프로젝트를 실행하면 톰캣 서버 버전을 선택하라는 대화상자가 나온다. 여기서 자신이 원하는 버전을 선택하면 된다. 나는 8.5 버전을 선택했는데 다음과 같은 에러가 뜨면서 버튼들도 비활성화되었다.

<span style="background-color:yellow; color:red;">ERROR : The server does not support version 4.0 of the J2EE Web module specification.</span>

![2020-12-20-eclipse-error-switch_module_version](/assets/images/jsp/2020-12-20-eclipse-error-switch_module_version-01.png)



## :0 Think

에러 메시지를 해석해보면 **톰캣 서버가 웹 모듈 4.0 버전의 스펙을 지원하지 않는다**고 한다. 이게 무슨 말일까? 구글링을 통해서 나름대로 정리를 해봤다.

* `Dynamic Web Module` 은 동적 페이지를 만들어서 보여주기 위한 `Servlet API Version` 을 의미한다.
* 이러한 사양을 `Servlet Spec(Specification)` 이라고 한다.
* 서블릿 스펙에 맞는 톰캣 버전은 정해져 있다.
  * 버전별 스펙들은 아파치 공식 홈페이지에서 확인할 수 있다.
    * http://tomcat.apache.org/whichversion.html
* 따라서 각 버전들을 정확하게 매핑해줘야 한다.



## :) Solution

1. Dynamic Web Module Version 확인하기

2. Servlet Spec에 맞는 Tomcat Version 확인하기

3. Dynamic Web Module Version 변경하기

### Step 1. Dynamic Web Module Version 확인하기

[Project Properties] - [Project Facets]에서 Dynamic Web Module 버전을 살펴보니 4.0이다. 

> Project Facets
>
> * 프로젝트의 기본 형태를 지정해 준다.
> * 기본 형태(웹인지? maven 인지? Spring 인지? 등등)에 따라 jdk를 맞춰줘야 하는데, 웹의 경우에는 서버 버전까지 맞춰줘야 한다.

![2020-12-20-eclipse-error-switch_module_version-02](/assets/images/jsp/2020-12-20-eclipse-error-switch_module_version-02.png)



### Step 2. Servlet Spec에 맞는 Tomcat Version 확인하기

아파치 공식 홈페이지에서 버전별 스펙 표를 보면 `Servlet Spec 4.0` 은 `Tomcat Version 9.0.x` 와 매핑된다. 따라서 `Tomcat Version 8.5.x` 와 매핑 시키기 위해서는 `Servlet Spec 3.1` 로 버전을 낮춰야 한다.

![2020-12-20-eclipse-error-switch_module_version-03](/assets/images/jsp/2020-12-20-eclipse-error-switch_module_version-03.png)



### Step 3. Dynamic Web Module Version 변경하기

알맞은 Servlet Spec 버전을 알았으니 Project Facets 설정으로 돌아와서 4.0 -> 3.1 버전으로 낮춰보자. 그런데, 여기서 문제가 생긴다. (문제가 안 생긴다면 적용하고 즐기면 된다.)

![2020-12-20-eclipse-error-switch_module_version-04](/assets/images/jsp/2020-12-20-eclipse-error-switch_module_version-04.png)

<span style="background-color:yellow; color:red;">Cannot change version of project facet Dynamic Web Module to 3.1</span>

이처럼 3.1 버전으로 내리지도 못하는 상황이라면, 아래와 같이 해결하면 된다.

1. {해당 프로젝트 폴더}/.settings/org.eclipse.wst.common.project.facet.core.xml 파일을 편집한다.
    * `.settings` 폴더가 안보여요 ㅠㅠ
        * 윈도우즈의 경우 보기 옵션에서 `숨김 파일 표시` 에 체크해 준다.
        * 맥의 경우 `command + shift + .`

2.  <installed facet="jst.web" **version="3.1"**/ >  4.0 -> 3.1 로 변경

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <faceted-project>
      <fixed facet="java"/>
      <fixed facet="wst.jsdt.web"/>
      <fixed facet="jst.web"/>
      <installed facet="java" version="1.8"/>
      <installed facet="jst.web" version="3.1"/>
      <installed facet="wst.jsdt.web" version="1.0"/>
    </faceted-project>
    ```

3. 프로젝트를 새로 고침 해주면 자동으로 버전이 3.1로 바뀌어 있을 것이다.



## 마치며

다이나믹 웹 프로젝트 생성 시에 dynamic web module version이라는 섹션에서 모듈 버전을 선택할 수 있었다. 항상 개발 환경에 맞게 자동으로 잡아줘서 그냥 냅뒀었는데 중요한 부분이었구나!

그리고 Project Facets에서 Dynamic web module에 체크를 해주고 알맞은 스펙의 버전을 선택해 주면 기본 java 프로젝트도 동적 웹 프로젝트로 변환해서 실행할 수 있다는 것을 알게 되었다.

