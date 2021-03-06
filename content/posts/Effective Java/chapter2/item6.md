---
title: "아이템6 - 불필요한 객체 생성을 피하라."
date: "2020-04-17 06"
template: "post"
draft: false
category: "Effective Java"
tags:
  - "Java"
  - "effectiveJava"
description: "2장. 객체 생성과 파괴 - 불필요한 객체 생성을 피하라."
---

### 아이템 6 - 불필요한 객체 생성을 피하라

똑같은 기능의 객체를 매번 생성하기보다는 객체 하나를 재사용하는 편이 나을 때가 많다. 재사용은 빠르고 세련되다. 불변 객체는 언제든 재사용할 수 있다.

### 문자열 객체 생성

```java
String name = new String("hanul");   // (1)
String name2 = "hanul";			   	 // (2)
```

String을 new를 이용해서 생성하면(1) 항상 새로운 객체를 만들게 된다. 이러한 경우 쓸데없은 인스턴스가 수백만 개 만들어질 수도 있다. 따라서, (2) 의 코드와 같이 문자열 리터럴을 사용하여 객체를 생성하는 것이 좋은 방법이다. 같은 가상 머신 안에서 이와 똑같은 문자열 리터럴이 존재하면 그 리터럴을 재사용한다.

### 정적 팩터리 메서드를 사용
Boolean(String) 생성자 대신 Boolean.valueOf(String) 팩터리 메서드를 사용하는 것이 좋다. (생성자는 자바 9에서 deprecated 지정됨). 생성자는 호출할 때마다 새로운 객체를 만들지만, 팩터리 메서드는 전혀 그렇지 않다.

```java
Boolean true1 = Boolean.valueOf("true");
Boolean true2 = Boolean.valueOf("true");

System.out.println(true1 == true2);
```

### 무거운 객체
만드는데 메모리나 시간이 오래 걸리는 객체 즉 "비싼 객체"를 반복적으로 만들어야 한다면 static final 로 초기에 캐싱해두고 재사용하는 것이 좋다.

정규 표현식으로 예제로 살펴보자. 문자열이 로마 숫자를 표현하는지 확인하는 코드는 다음과 같다.

```java
static boolean isRomanNumeral(String s) {
    return s.matches("^(?=.)M*(C[MD]|D?C{0,3})(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```

String.matches가 가장 쉽게 정규 표현식에 매치가 되는지 확인하는 방법이긴 하지만 성능이 중요한 상황에서 반복적으로 사용하기에 적절하지 않다.

String.matches는 내부적으로 Pattern 인스턴스를 만들어 쓰는데 이 인스턴스는 한 번 쓰고 버려져서 곧바로 가비지 컬렉션 대상이 된다. 즉 비싼 객체다.

성능을 개선하려면 Pattern 객체를 만들어 컴파일하고 재사용하는 것이 좋다.

```java
public class RomanNumber {

    private static final Pattern ROMAN = Pattern.compile("^(?=.)M*(C[MD]|D?C{0,3})(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

    static boolean isRomanNumeral(String s) {
        return ROMAN.matcher(s).matches();
	}
}
```

### 같은 인스턴스를 대변하는 여러개의 인스턴스를 생성하지 말자
어댑터는 실제 작업은 뒷단 객체에 위임하고 자신은 제2의 인터페이스 역할을 해주는 객체이다. 어댑터는 뒷단 객체만 관리하면 되므로, 뒷단 객체 하나 당 하나씩만 만들어지면 된다.

예를 들어 Map 인터페이스의 keySet 메서드는 호출할 때마다 새로운 Set 인스턴스를 반환하지 않는다.

```java
Map<String, String> phoneBook = new HashMap<>();
phoneBook.put("hanul", "010-1234-1234");
phoneBook.put("kim", "010-4321-4321");
Set<String> keySet1 = phoneBook.keySet();
Set<String> keySet2 = phoneBook.keySet();

System.out.println(keySet1 == keySet2); // true
System.out.println(keySet1.size() == keySet2.size()); // true
keySet1.remove("kim");
System.out.println(phoneBook.size()); // 1
```

### 오토박싱
오토박싱은 기본타입과 레퍼런스 타입 간에 자동으로 상호변환해주는 기술이다. 개발자 입장에서 구분을 흐리게해서 편하게 사용가능하게 해주지만, 그 경계가 완전히 없어진 것은 아니다.

```java
Long sum = 0L;
for (long i = 0; i <= Integer.MAX_VALUE; i++) {
    sum += i;
}
```

불필요한 박싱/언박싱은 불필요한 메모리 할당, 재할당을 반복할 수 있다. 꼭 박싱 타입이 필요한 경우가 아니라면 기본타입을 사용하도록 하자.