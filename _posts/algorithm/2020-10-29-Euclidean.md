---
toc: true
toc_sticky: true
title: "최대 공약수(GCD) 알고리즘 - 유클리드 호제법"
excerpt: ""
categories:
  - Algorithm
tags:
  - Algorithm
---

## 서론

프로그래머스에서 [멀쩡한 사각형](https://programmers.co.kr/learn/courses/30/lessons/62048)이라는 문제를 풀다가 거의 두 시간 만에 규칙을 찾아냈는데, 그 규칙이 바로 최대 공약수였다. 최대 공약수와 최소 공배수의 개념은 알지만 이를 구현해 본 적이 없었기에 이번 포스팅을 통해서 정확하게 짚고 넘어가야겠다.

## 최대 공약수(GCD) 구하기

최대 공약수(`GCD, Greatest Common Divisor`)란 두 자연수의 공통된 약수 중에서 가장 큰 수를 의미한다.

* ex) GCD(8, 12) = 4

### 기존의 GCD 구하는 방법 (Brute force, 무차별 대입)

1. 두 자연수 중에서 작은 자연수를 선택
  * min(8, 12) => 8선택
2. 2부터 8까지 모든 수로 두 수를 나누다가 동시에 0으로 떨어지는 가장 큰 수를 구한다.

```java
public static int gcd(int a, int b) {
  int result = 0;
  for(int i = 1; i <= b; i++) {
    if(a % i == 0 && b % i == 0) result = i;
  }
  return result;
}
```

* 시간복잡도 : O(N)

### 유클리드 호제법(Euclidean Algorithm)

유클리드 호제법이란 간단하게 `O(logN)`의 시간복잡도로 두 자연수의 최대 공약수를 구할 수 있는 알고리즘이다.

> 기존의 방법도 `O(N)`의 시간복잡도로 나쁘지는 않지만 효율을 높히기 위해 이 알고리즘을 사용한다.

* **a를 b로 나눈 나머지를 r이라고 할때 (단, a > b) a와 b의 최대 공약수는 b와 r의 최대 공약수와 같다는 성질을 이용한다.**
* 이런 성질을 통해서 b와 r의 최대 공약수 r0를 구하고, 다시 r을 r0로 나눈 나머지를 구하는 과정을 반복하여(`recursive`) 나머지가 0이 되었을 때의 그 몫이 a와 b의 최대 공약수이다.
* 이를 수식으로 나타내면 아래와 같다.

	```java
	GCD(A, B) = GCD(B, A%B)
		if A%B = 0 -> GCD = B
			else GCD(B, A%B)
	```

> CODE

```java
public static int gcd(int a, int b) { // a > b 일때
  if(b == 0) return a;	// gcd를 찾았다면 그 몫을 return
  else return gcd(b, a % b);
}
```



## 최소 공배수(LCM) 구하기

최소 공배수(`LCM, Least Common Muliple`)란 두 자연수의 공통된 배수 중 가장 작은 수를 의미한다. 

* ex) LCM(8, 12) = 24

* 최소 공배수 = 두 자연수의 곱 / 최대 공약수 ( LCM = a x b / GCD )

> CODE

```java
public static int lcm(int a, int b, int gcd) {
  return (a * b) / gcd
}
```

