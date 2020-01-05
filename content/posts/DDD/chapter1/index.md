---
title: "������ �� ����"
date: "2020-01-04"
template: "post"
draft: false
path: "/DDD/chapter1/"
category: "domaindrivendesign"
description: "������ �� ������ �����ϰ� ������ �����̴�."
slug: "��Ű��ó-����"
---

## �������̶�?
  - �ڵ�ȭ�� �����Ͻ��� ���Ǽ����� ����. ������ �ذ��ϰ��� �ϴ� ���� ����.
  - ������� �¶��� ���� �ý����� �����Ѵٰ� �� ��, ����Ʈ����� �ذ��ϰ��� �ϴ� ���� ������ `�¶��� ����`�� �������� �ȴ�.
  - �������� �������� ���� ���������� ���� �� �ִ�.
    - �¶��� ������ ���� ������ : �ֹ�, īŻ�α�, ȸ��, ����, ��� ��
  - Ư�� �������� �����ؾ� �� ��� ����� ���������� �ʴ´�.
    - �ܺ� ��ü�� �ý����� ����Ѵ�. (�¶��� ���θ��� ��ü������ ��� �ý����� �����ϱ� ���ٴ� �ܺ� ��۾�ü�� �ý����� ����Ѵ�.)
  - �����θ��� ������ ���� �������� �����ϴ� ���� �ƴϴ�.
    - ����� �Ը� ��� ���� �޶�����.
  
## ������ ��
- Ư�� �������� ���������� ǥ���� ��.
- ������ ���� �������� �����ϴ� ��ɰ� �ֿ� ������ ������ �����Ѵ�.
- �������� �𵨸��ϴµ��� ��ü, ���� ���̾�׷�, ���� ���� �� ��� ���̵� ������ ����. �𵨸� ����� ��� ���ذ����ڰ� �����ϴ� ���� �߿��ϴ�.
- ������ ���� ������ ��ü�� �����ϱ� ���� ���� ���̴�. ������ ���� ���� ���� ���� �ʿ��ϴ�.

## ������ �� ����
- ����� �������̽�(UI) or ǥ��(Presentation)
  - ������� ��û�� ó�� �� ����� ������
  - ����ڴ� ���� �����Ӹ� �ƴ϶� �����ϴ� �ܺ� �ý����� �� ���� ����
- ���� (Application)
  - ����ڰ� ��û�� ����� ����
  - ���� ������ ���� �������� �ʰ�(!!) ������ ������ ������ ��Ģ���� ������ ������
- ������ (Domain)
  - �ý����� ������ �������� ��Ģ�� ����
- ������Ʈ��ó (Infrastructure)
  - �����ͺ��̽�, �޽�¡ �ý��� �� �ܺ� �ý��۰��� ������ ó��

## ������ �� ����
������ �� ������ �������� �����غ��� ���� ������ �ֵ� ������ �ٽ��� �������� �����غ���.
������ �ֵ� ������ �ٽ��� ���� �� ��ġ�� ���� �ʰ� ����Ʈ���� ���߿� �⿩�ϵ��� `�������� �� ǥ���� ���� �����`, `�Բ�, ����ؼ� �� ���� �ٶ󺸴�` ���̴�.


### ������ �� �����̶�?
  - �������� �𵨸��� �� �⺻�� �Ǵ� �۾��� ���� �����ϴ� `�ٽ� �������, ��Ģ, ���`�� ã�� ���̴�.
  - �䱸������ �м��Ͽ� �޼���, �����͸� �����ϰ� ã�Ƴ��� ���̴�.


### ������ �ֵ� ������ �ٽ��� ��Ű�鼭 ������ �� ������ �ϱ� ���ؼ���,
  - ��� ����� �� ������ �����ؾ� �Ѵ�. (������ ������, ������, �м��� ��)
  - ���� �׻� ��������� ������ �ʿ��Ѵ�.
    - ������ �������� ������ �� �ִ°�?
    - �ٸ� �����ڰ� �ش� �������� ������ �� �ִ°�?

�̷��� ������� ���� `����ȭ`�� ���� �����Ͽ�, ������ ���� ������ �� �ֵ��� �Ѵ�.

## ��ƼƼ�� ���
- ������ ������ ���� ũ�� ��ƼƼ(Entity)�� ���(Value)�� ���еȴ�.

## ��ƼƼ(Entity)
- ��ƼƼ�� �����ϱ� ������ `�ĺ���`�� ���´�.
- ��ƼƼ �ĺ��ڰ� ���� �� ���� ��ƼƼ�� ���� ��ƼƼ��� �Ǵ��� �� �ְ� ��ƼƼ�� �Ӽ��� �ٲٰų� �����ϱ� �������� �����ȴ�.
- �̸� �������� equals, hashCode �޼ҵ带 �Ʒ��� ���� ������ �� �ִ�.
```java
public class Entity {
    private String identifier;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (obj.getClass() != Entity.class) return false;
        Entity other = (Entity) obj;
        if (this.identifier == null) return false;
        return this.identifier.equals(other.identifier);
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + ((identifier == null) ? 0 : identifier.hashCode());
        return result;
    }
}
```
## ��ƼƼ�� �ĺ��� ����
- ��ƼƼ �ĺ��ڴ� Ư�� ��Ģ �Ǵ� ���� ����� ���� ���� ����� �޶�����.
- Ư�� ��Ģ�� ���� ����
  - ��ǥ������ ��¥�� ������ ���ýð� ���� �̽��� �Ű����Ѵ�.
- `8-4-4-4-12` ������ 16���� ���ڿ� ������ UUID ��� (Java�� ��� `UUID` Ŭ���� Ȱ��)

```java
UUID uuid = UUID.randomUUID();

// 615f2ab9-c374-4b50-9420-2154594af151 �� ���� ������ ���ڿ� ����
String identifier = uuid.toString();
```

- ���� ���� �Է�
  - ȸ�� ID, �̸��� ��
- �Ϸù�ȣ ���
  - ����Ŭ�� ������ ������Ʈ, MySQL Auto Increment �÷� ��

## ���(Value)
���������� ������ �ϳ��� ǥ���� �� ����Ѵ�. ������� �Ʒ��� ���� ��������� �ִ� ��츦 ����.
```java
public class ShippingInfo {
    // �޴� ���
    private String receiverName;
    private String receiverPhoneNumber;
    // �ּ�  
    private String shippingAddress1;  
    private String shippingAddress2;  
    private String shippingAddress3;  
}
```
- `receiverName`, `receiverPhoneNumber` �� ��� ���� �ٸ� ������ ǥ���ϰ� ������, ���������δ� **�޴� ���**�̶�� �ϳ��� ������ ǥ���ϰ� ������ �Ʒ��� ���� ������ �� �ִ�.
- `Receiver`��� Ŭ������ ��� Ÿ���� �����Ͽ� ������ ������ ���� �� �� ǥ���� �� �ִ�. 
```java
public class Receiver {
    private String name;
    private String phoneNumber;

    public Receiver(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }

    public String getName() {
        return this.name;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }
}
```
- ���������� `shippingAddress1`, `shippingAddress2`, `shippingAddress3`�� ���������� **�ּ�**��� �ϳ��� ������ ǥ���ϰ� �ִ�.
```java
public class Address {
    private String address1;
    private String address2;
    private String zipcode;

    public Address(String address1, String address2, String zipcode) {
        this.address1 = address1
        this.address2 = address2
        this.zipcode = zipcode
    }
}
```
- ��� Ÿ���� Ȱ���� `ShippingInfo`�� �ٽ� �����ϸ� ���� �� ��ü�������̰� ��Ȯ�ϰ� �����ȴ�.
```java
public class ShippingInfo {
    private Receiver receiver;
    private Address address;

    ...
}
```
- ���Ÿ���� �� ���� �Ӽ��� �ϳ��� ���� �ʴ��� �ǹ̸� ��Ȯ�ϱ� ���� ����ϱ⵵ �Ѵ�.
- �Ʒ� ������ ��� `price`, `quantity`, `amounts` ���� ����, ����, �ݾ��� �ǹ��ϴµ� ���ݰ� �ݾ��� **��**�� �ǹ��ϴ� ���̴�.
```java
public class OrderLine {
    private int price;
    private int quantity;
    private int amounts;
    ...
}
```
- �̸� Money��� ����� ����� ����ϸ� ������ ���� ǥ���� �� �ִ�.
```java
public class Money {
    private int value;

    public Money(int value) {
        this.value = value;        
    }

    public int getValue() {
        return this.value;
    }
}
// int ��� Money Ÿ���� ����Ͽ� ������ �Ӽ��� �ǹ��ϴ� �ٸ� ���� ��Ȯ�ϰ� ǥ��
public class OrderLine {
    private Money price;
    private int quantity;
    private Money amounts;  
}
```
- ���� �̷��� ����� �����ϸ� �ش� ����� ǥ���ϴ� �������� ��Ģ �Ǵ� ����� �߰��� ������ �� �ִ�.
- �� ����� ����ϸ� ������ �ڵ忡 ������ ���� �ǹ̸� �� �� ǥ���� �� �ְԵȴ�.
```java
public class Money {
    private int value;

    public Money(int value) {
        this.value = value;        
    }

    public int getValue() {
        return this.value;
    }

    public Money add(Money money) {
        return new Money(this.value + money.value);
    }

    public Money multiply(Money money) {
        return new Money(this.value * money.value);
    }    
}
```
```java
public class OrderLine {
    private Money price;
    private int quantity;
    private Money amounts;  

    public Money calculateAmounts() {
        // �ڵ尡 �� ����̶�� �ǹ̸� ���Ե�
        return price.multiply(amounts);
    }
}
```
```java
public class OrderLine {
    private int price;
    private int quantity;
    private int amounts;  

    public int calculateAmounts() {
        // �ڵ忡 �� ����� �ǹ̴� ���� ���� ���� Ÿ�� ������ ��
        return price * amounts;
    }
}
```
- ��� ��ü�� ���� �Һ�(Immutable)Ÿ������ �����Ͽ� ���� ����� �������� �ʴ´�.
  - ��� ��ü�� setter ���� ���� ����� �����ϸ� �Ǽ��� ��Ÿ ������ ��� ��ü�� ���� ���� ���ɼ��� ����� ���ռ��� ������ ��Ȳ�� �߻��Ѵ�.
  - ������ �ַ� ������ ������ ���ο� ��� ��ü�� �����ϴ� ����� �̿��Ѵ�.
  - �̸� ���� ���� ������(�ܺ��� ��ȭ�� ���� �޴� ���⵵�� ����)�� �����忡 ���� (Thread Safe)�ϴٴ� Ư¡�� ���Եȴ�.
- �� ���� ��ƼƼ ��ü�� ������ �Ǻ��ϱ� ���� `�ĺ���`�� ����Ѵٸ�, ��� Ÿ���� `Ÿ�� �� ��� �Ӽ�`�� ������ ���ؾ��Ѵ�.

## ��ƼƼ �ĺ��ڿ� ����Ÿ��
- ��ƼƼ�� �ĺ��ڴ� String�� ��찡 ����. ��ƼƼ �ĺ��ڸ� ����Ÿ���� ���� ������ ������ҷ��� �ǹ̰� �� �巯������ �Ͽ� �������� ������ �ִ�.
```java
public class Order {
    private String id;
}
```

```java
// Ŭ������ ���� id �ʵ尡 Order ��ƼƼ�� �ĺ������� ǥ��
public class Order {
    private OrderNo id;
}
```

## ������ �𵨿� set �޼��� ���� �ʱ�
������ �� Vo, Dto �� Ŭ������ ����� ���������� getter/setter�� ����� ��찡 ����. ��ó�� ������ �𵨿��� �ǹ̾��� �߰��ϴ� set�޼ҵ�� ��� �������� �߻���Ų��.
- ������ ���� �ٽ� ����, �ǵ��� �ڵ忡�� ��������Ѵ�.
```java
public class Order {
    ...
    public void setShippingInfo(ShippingInfo shippingInfo) { ... }
    public void setOrderState(OrderState state) { ... }
}
```
setter�� �߰��ص� ���� Ŭ�������� `setShippingInfo`, `setOrderState`�� �ܼ��� `����� ���� ����(set)`, `�ֹ����� ����(set)`�̶�� �ǹ̸� ������ �ִ�. �ݸ� �Ʒ� �ڵ带 ����.
```java
public class Order {
    ...
    public void changeShippingInfo(ShippingInfo shippingInfo) { ... }
    public void completePayment() { ... }
}
```
`changeShippingInfo`, `completePayment`��� �޼ҵ� �̸��� ���� `����� ���� ����`, `���� �Ϸ�`��� ������ �ǹ̸� ���� �� �ִ�.

- �� �ٸ� ������ ������ ��ü�� ������ �� ������ ���°� �ƴ� ���� �ִ�.
```java
    // Order�� �������Ǵ� �������� order�� �Ϻ����� ����.
    Order order = new Order();

    // set���� ��� ������ �����ؾ� ��.
    order.setOrderLine(line);
    order.setShippingInfo(info);

    // �ֹ����� ������ �������� ���� ���¿��� �ֹ��� �Ϸ� ��.
    order.setState(OrderState.PREPARING);
```
������ ��ü�� �ҿ����� ���·� ���Ǵ� ���� �������� ���� ������ �ʿ��� ���� ������ �־�� �Ѵ�.
-> `������`�� ���� �ʿ��� �����͸� ��� �޾ƾ� �Ѵ�.
```java
public class Order {
  public Order(Orderer orderer, List<OrderLine> orderLines,
  ShippingInfo shippingInfo, OrderState state) {
      setOrderer(orderer);
      setOrderLines(orderLines);
      setShippingInfo(shippingInfo);
      ... // �� �� ����
  }

  private setOrderer(Orderer orderer) {
      if (orderer == null) {
          throw new IllegalArgumentException("no orderer");
      }
      this.orderer = orderer;
  }

  private void setOrderLines(List<OrderLine> orderLines) {
      verifyAtLeastOneOnMoreOrderLines(orderLines);
      this.orderLines = orderLines;
      calulateTotalAmounts();
  }

  private void verifyAtLeastOneOnMoreOrderLines(List<OrderLine> orderLines) {
      if (orderLines == null || orderLines.isEmpty()) {
          throw IllegalArgumentException("no orderLines");
      }
  }

  private void calulateTotalAmounts() {
      this.totalAmounts = orderLines.stream().mapToInt(
          x -> x.getAmounts()
      ).sum();
  }
}
```
�� �ڵ��� set �޼���� ���� ������ private�̱� ������ �ܺο��� �����͸� ������ �������� set �޼��带 ����� �� ����.
- Immutable�� ��� Ÿ���� ����ϸ� �ڿ������� setter�� �������� �ʰԵȴ�. 
- Ư���� ������ ���ٸ�, ���� Ÿ���� �Һ����� �����Ѵ�.

## ������ ���
������ �� �ڵ忡 �ݿ����� ������ �ڵ带 �ؼ��ؾ� �ϴ� �δ��� �����.
```java
Public OrderState {
  STEP1, STEP2, STEP3... 
}
```
STEP1, STPE2�� ���� ���� �ǹ����� �� �� ������, �ڵ带 �м��ϰ� �����ϴµ� �ð��� �ʿ��ϴ�.
������ PAYMENT_WAITING, PREPARING, SHIPPED �� ���� �����ο��� ����ϴ� �� �ִ��� �ݿ��Ѵٸ�
���� ������ �� �ְ� �ǹ̸� ��ȯ�ϴ� �������� �߻��ϴ� ���׵� ���ҽ�ų �� �ִ�.
�׷��Ƿ� ������ �� �˸��� �ܾ ã�� �ð��� �Ʊ���������� ������ ������ �� ����ϱ� ���� �������.

### ����
> [�ֹ���,��DDD Start!��, ���ؼ�(2016)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=84000742)