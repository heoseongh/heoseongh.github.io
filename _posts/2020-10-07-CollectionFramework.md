---
title: "컬렉션 프레임워크란?"
excerpt: "Collection Framework의 전체적인 구조에 대해서 알아보자."

categories:
  - Java
tags:
  - Java
  - collection
  - framework
---
- 프로그램 구현에 필요한 자료구조와 알고리즘을 구현해 놓은 라이브러리
- java.util 패키지에 포함. (JDK1.2부터 제공)
- 개발에 소요되는 시간을 절약하고 최적화된 라이브러리를 사용할 수 있음
- **Collection 인터페이스**와 **Map 인터페이스**로 구성됨

## # Collection 인터페이스(List, Set)

- 하나의 객체의 관리를 위해 선언된 인터페이스로 필요한 기본 메서드가 선언되어 있음
- 하위에 **List, Set** 인터페이스가 있음

![이미지](https://raw.githubusercontent.com/heoseongh/heoseongh.github.io/main/assets/images/java/2020-10-07-collection-1.png)

### List 인터페이스

- 순서가 있는 자료를 관리할때 사용하며 중복을 허용한다.
- 이 인터페이스를 구현한 클래스는 **ArrayList, Vector, LinkedList, Stack, Queue** 등이 있다.

### Set 인터페이스

- 순서가 정해져 있지 않은 자료를 관리할 때 사용하며 중복을 허용하지 않는다.
- 이 인터페이스를 구현한 클래스는 **HashSet, TressSet** 등이 있다.

## # Map 인터페이스

- 쌍으로 이루어진 객체를 관리하는데 필요한 여러 메서드가 선언되어 있음
- Map을 사용하는 객체는 key-value 쌍으로 되어 있고 key는 중복될 수 없음
- 이 인터페이스를 구현한 클래스는 **Hashtable, HashMap, TreeMap, Properties** 등이 있다.

![이미지](https://raw.githubusercontent.com/heoseongh/heoseongh.github.io/main/assets/images/java/2020-10-07-map.png)

