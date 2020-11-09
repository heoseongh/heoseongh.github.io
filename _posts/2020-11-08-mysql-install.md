---
title: "MySQL 설치와 셋팅 - homebrew"
excerpt: ""
categories:
  - MySQL
tags:
  - MySQL
---

## MySQL Install

```bash
$ brew search mysql     # mysql 버전을 검색해보자
$ brew install mysql    # 버전을 생략하면 최신 버전으로 설치됨
$ brew list             # 잘 설치되었나 확인
```

## MySQL Setting

```bash
$ mysql.server start		# 서버 구동(서버를 켜야 환경설정으로 진입 가능)
$ mysql_secure_installation	# 환경설정 명령어
```

Q. Woult you like to setup VALIDATE PASSWORD component?

* Y : 복잡한 비밀번호 사용

* **N : 단순한 비밀번호 사용**

Q. Remove anonymous users?

* **Y : $ mysql -uroot 처럼 -u 옵션이 필수이다.**

* N : $ mysql 만으로 접속이 가능하다.
<!--  -->
Q. Disallow root login remotely? n

* Y : 원격 접속 거부

* **N : 원격 접속 허용**

Q. Remove test database and access to it?

* **Y : MySQL의 Test DB 삭제**

* N : 유지

Q. Reload privilege tables now? y

* **Y : 설정 내용을 적용할건지?**

## MySQL 접속

```bash
$ mysql -uroot -p
Enter password: #### 설치시 기입했던 root password 입력
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.22 Homebrew
...
mysql> exit 		# 종료
Bye
```
