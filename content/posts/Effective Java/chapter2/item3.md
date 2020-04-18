---
title: "아이템3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라."
date: "2020-04-17 03"
template: "post"
draft: false
category: "Effective Java"
tags:
  - "Java"
  - "effectiveJava"
description: "2장. 객체 생성과 파괴 - private 생성자나 열거 타입으로 싱글턴임을 보증하라."
---

### 아이템 3 - private 생성자나 열거 타입으로 싱글턴임을 보증하라.

싱글턴이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다. 싱글턴의 전형적인 예로는 함수와 같은 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있다. **클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기 어려워 질 수 있다.** 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글턴이 아니라면 인스턴스를 가짜 구현으로 대체할 수 없기때문이다.

싱글턴을 만드는 방식은 보통 둘 중 하나다. 두가지 방식 모두 private 생성를 통하여 인스턴스화를 막는다.

**첫 번째, public static final 필드 방식의 싱글**

```java
public class Single {
    public static final Single INSTANCE = new Single();
    
    // private를 사용한 인스턴스화 방지
    private Single {} ();
}
```

private 생성자는 public static final 필드인 Single.INSTANCE를 초기화할 때 딱 한 번만 호출된다. public이나 protected 생성자가 없으므로 Single 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에 하나뿐임이 보장된다.

public static final 필드 방식의 싱글턴의 장점은 첫 번째 해당 클래스가 싱글턴임이 API에 명백히 드러난다는 것이다. public static 필드가 final이니 절대로 다른 객체를 참조할 수 없다. 두 번째 장점은 간결함이다. 

**두 번째, 정적 팩터리 방식의 싱글턴**

```java
public class Single {
    private static final Single INSTANCE = new Single();
    public static Single getInstance() {
        return INSTANCE;
    }
    
    // private를 사용한 인스턴스화 방지
    private Single {} ();
}
```

Single.getInstance는 항상 같은 객체의 참조를 반환하므로 제2의 Single 인스턴스란 만들어지지 않는다.

정적 팩터리 방식의 싱글턴의 장점은 첫 번째 장점은 API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다는 점이다. 유일한 인스턴스를 반환하던 메서드가 스레드별로 다른 인스턴스를 넘겨주게 할 수 있다. 두번 째 장점은 원한다면 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있다는 점이다.세번 째 장점은 정적 팩터리의 메서드 참조를 공급자로 사용할 수 있다는 점이다. 위에 장점들이 필요하지 않다면 public 필드 방식이 좋다.

예외는 단 한가지가 있다. 권한이 있는 클라이언트는 리플렉션 API인 AccessibleObject.setAccessible을 사용하여 private 생성자를 호출할 수 있다. 이러한 공격을 방어하려면 생성자를 수정하여 두 번째 객체가 생성되려 할 때 예외를 던지게 하면 된다.

싱글턴 클래스를 직렬화하려면 단순히 Serializable을 구현한다고 선언하는 것만으로는 부족하다. 모든 인스턴스 필드를 일시적\(transient\)이라고 선언하고 readResolve 메서드를 제공해야한다.

readResolve는 새로운 역 직렬화 된 인스턴스를 반환하기 때문에 아래와 같이 기존에 인스턴스를 반환해 줘야한다.

```java
private Object readResovle() {
    // 기존 인스턴스를 반환하고 가짜인 새로운 인스턴스는 가비지 컬렉션에 맡긴다.
    return INSTANCE;
}
```

**세 번째, 열거 타입 방식의 싱글턴 - 바람직한 방법**

```java
public enum Single {
    INSTANCE:
}
```

public 필드 방식과 비슷하지만, 더 간결하고, 추가 노력 없이 직렬화 할수 있고, 아주 복잡한 직렬화 상황이나 리플렉션 공격에도 제2의 인스턴스가 생기는 일을 완벽히 막아준다. **대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.**