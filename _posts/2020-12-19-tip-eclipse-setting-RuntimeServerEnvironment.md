---
toc: true
toc_sticky: true
title: "이클립스 서버 런타임 환경 설정하기: 아파치 톰캣"
excerpt: "이클립스에서 톰캣 설정을 마스터해보자"

categories:
  - JSP
tags:
  - JSP
  - Eclipse
  - Setting
  - Tomcat
--- 

## :( Trouble

이클립스에 웹 프로젝트를(.war) import 했더니 다음과 같은 에러가 떴다.

<span style="background-color:yellow; color:red;">"javax.servlet.http.HttpServlet" was not found on the Java Build Path</span>

![2020-12-19-error-server-runtime-environment](/assets/images/jsp/2020-12-19-error-server-runtime-environment.png)



## :) Shoot

이클립스에서 서버 런타임 환경 설정이 되어있지 않아서 발생하는 에러이다. 이는 아래와 같은 절차로 해결이 가능하다.

> 나는 외부 프로젝트를 임포트할 때 생겼던 문제였지만 이 방법으로 기본적인 톰캣 문제는 해결이 가능하니 알아 두면 좋다.

* 프로젝트가 톰캣을 인식할 수 있도록 **클래스 패스에 서버 런타임 추가**하기
* 런타임 서버이 자동으로 인식되지 않으면 수동으로 톰캣 패스 잡아주기
* 프로젝트에 톰캣 서버 추가하고 빌드하기

### 클래스 패스에 서버 런타임 추가

1. 빌드 패스 설정으로 진입.

   * 프로젝트의 콘텍스트 메뉴에서 [Build Path] - [Configure Build Path...]

   ![2020-12-19-server-runtime-environment-01](/assets/images/jsp/2020-12-19-server_runtime_environment-01.png)

2. 라이브러리에 서버 런타임 추가

   * [Java Build Path] - [Library] - [Add Liarary] - [Server Runtime]

   ![2020-12-19-server-runtime-environment-02](/assets/images/jsp/2020-12-19-server_runtime_environment-02.png)


3. 서버 런타임을 선택해주면 클래스 패스에 추가된다. 헌데.. 나는 왜 목록이 없는것인가?

   ![2020-12-19-server-runtime-environment-03](/assets/images/jsp/2020-12-19-server_runtime_environment-03.png)

   톰캣을 깔고 나서 환경 변수 설정을 해줬다면 자동으로 잡힐텐데 나는 왜 안보이는 것인가.. 이럴 때는 수동으로 톰캣이 설치되어있는 경로를 지정해주면 된다.



### 수동으로 톰캣 패스 잡아주기

1. 이클립스 환경설정(`command + ,`)에 들어가서 아래와 같은 경로로 진입한다.

   ![2020-12-19-server-runtime-environment-04](/assets/images/jsp/2020-12-19-server_runtime_environment-04.png)

2. [Browse...] 에서 직접 톰캣이 설치되어있는 경로를 잡아준다.

   ![2020-12-19-server-runtime-environment-05](/assets/images/jsp/2020-12-19-server_runtime_environment-05.png)

   * HomeBrew로 설치한 경우
     * `/usr/local/Cellar/tomcat@8/8.5.x/libexec` 
   * 윈도우의 경우
     * `C:/Program Files/Apache Software Foundation/Tomcat 8.5.x`

3. 쨘~ 이제 서버 런타임 환경에 톰캣이 잡힌다.

   ![2020-12-19-server-runtime-environment-06](/assets/images/jsp/2020-12-19-server_runtime_environment-06.png)

4. 이제 다시 프로젝트의 빌드 패스로 들어가서 서버 런타임(톰캣)을 추가해주면 된다.

   ![2020-12-19-server-runtime-environment-07](/assets/images/jsp/2020-12-19-server_runtime_environment-07.png)



### 서버 추가하고 빌드하기

1. 프로젝트 빌드시에 사용할 톰캣 서버를 추가해준다.

   ![2020-12-19-server-runtime-environment-09](/assets/images/jsp/2020-12-19-server_runtime_environment-08.png)

   서버가 성공적으로 추가되었다!

   ![2020-12-19-server-runtime-environment-09](/assets/images/jsp/2020-12-19-server_runtime_environment-09.png)

2. 톰캣 서버가 구동되면서 빌드가 정상적으로 된다. [Run As] - [Run on Server]

   ![2020-12-19-server-runtime-environment-10](/assets/images/jsp/2020-12-19-server_runtime_environment-10.png)



## 또 다른 문제의 시작

만약 빌드시 아래와 같은 에러가 뜨면서 Finish 버튼이 비활성된다면!!? 

글이 너무 길어지니 [**다음 포스팅**](/jsp/eclipse-error-switch_module_version)에서 해결해보도록 하겠다.

<span style="background-color:yellow; color:red;">ERROR : The currently selected server type does not support remote hosts</span>

![2020-12-19-server-runtime-environment-11](/assets/images/jsp/2020-12-19-server_runtime_environment-11.png)