---
title: "MySQL - JSP 연동시 에러모음"
excerpt: "JSP와 MySQL 연동시 발생하는 에러를 해결해보자."
categories:
  - MySQL
tags:
  - MySQL
---

# 서론
맥으로 바꾼 뒤 간만에 MySQL과 jsp를 연동해보았다. MySQL 8.0이 최신 버전이라 그런지 많은 에러들을 해결해야만 했다. 다음의 에러 메시지들을 보고 적절한 트러블슈팅을 해보자.

## Error :

모든 수정 후에는 1)프로젝트를 새로고침하고 2)서버를 다시 시작해주자. 

1. Project Refresh(`F5`) 

2. 서버 재실행(`command + option + R`)



<span style="background-color:yellow; color:red;">ClassNotFoundException: com.mysql.jdbc.Driver</span>

jdbc 드라이버를 찾지 못한것이다.

* Dynamic Web Project 의 경우 이클립스의 탐색기(project explorer)에서 `WebContent - WEB-INF - lib` 안에 jar 파일을 끌어다놓으면 된다.

* `/Library/Java/Home/lib/ext/` 에 `jar`파일을 넣으면 된다. 

  > homebrew로 설치한 경우는 경로가 조금 달라서 나는 `/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/jre/lib/ext/` 안에 넣어줬다.



<span style="background-color:yellow; color:red;">java.sql.SQLException: query_cache_size</span>

MySQL 8.0부터는 쿼리 캐시가 제거되었다고 한다. jdbc버전을 맞춰주자.

> 쿼리 캐시는 MySQL에만 존재하는 기능으로 성능 향상을 목적으로 사용한다.
>
> Key(sql문) - Value(질의결과)의 구조로 캐싱되며, 동일한 명령어를 받으면 매핑되는 값을 바로 리턴해준다.

* 에서 자신의 MySQL버전에 맞는 jar파일을 다운받아 압축을 풀어주고, 폴더 안에 있는 jar 파일을 알맞은 경로에 넣어준다.

  > Windows나 MacOS의 경우 `Platform Independent` 를 선택하고 `mysql-connector-java-[version].zip` 을 받는다. 



<span style="background-color:yellow; color:red;">java.sql.SQLException: The server time zone value 'KST' is unrecognized or represents more than one time zone.</span> You must configure either the server or JDBC driver (via the 'serverTimezone' configuration property) to use a more specifc time zone value if you want to utilize time zone support.

mysql-connector-java-5.1.X 버전부터 'KST' 타임존을 인식하지 못하는 이슈가 있다고 한다.

* URL에 serverTimezone 추가

  > jdbc:mysql://localhost:3306/[DB명]**?serverTimezone=Asia/Seoul**



<span style="background-color:yellow; color:red;">Loading class 'com.mysql.jdbc.Driver'. This is deprecated. The new driver class is 'com.mysql.cj.jdbc.Driver'.</span> The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.

위 메시지를 해석하면 : 로딩되고 있는 클래스는 'com.mysql.jdbc.Driver' 입니다. 이 것은 더이상 사용되지 않습니다. 새로운 드라이버 클래스는 'com.mysql.cj.jdbc.Driver' 입니다. (MySQL 8.0 이상부터 경로가 바뀌었나보다.)

* 드라이버 경로를 `com.mysql.cj.jdbc.Driver`로 바꿔준다.

  > Class.*forName*("**com.mysql.cj.jdbc.Driver**")

