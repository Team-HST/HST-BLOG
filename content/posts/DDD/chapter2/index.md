---
title: "��Ű��ó ����"
date: "2019-01-05"
template: "post"
draft: false
path: "/DDD/chapter2/"
category: "domaindrivendesign"
description: "��Ű��ó ���� �κ��� ���͵��ϰ� ������ �����̴�."
slug: "��Ű��ó-����"
---

## �� ���� ����
- ǥ��(UI)
  - ������� ��û�� �޾� ���� ������ �����ϰ� ���� ������ ó������� �ٽ� ����ڿ��� �����ִ� ������ �Ѵ�.
  - �� ���ø����̼� ���� �� ����ϴ� ������ MVC �����ӿ�ũ�� ǥ�� ������ ���� ����� �ش�ȴ�.
  - ǥ�� ������ ����ڴ� ����� ���� �ְ�, REST API�� ȣ���ϴ� �ܺ� �ý����� �����ִ�.
  - ǥ�� ������ HTTP ��û�� ���� ������ �ʿ��� ���·� ��ȯ�Ͽ� �����ϰ� ���� ������ ������ �ٽ� �ʿ��� HTTP ������ ����� �� ���������� �����Ѵ�.

- ����
  - ǥ�� ������ ���� ������� ��û�� ���޹��� ���� ������ ����ڿ��� ������ ����� �����Ѵ�.
  - ����� �����ϱ� ���� ������ ������ ������ ���� ���
  - ������ ���� �����ϱ⺸��, ������ �𵨿� ������ ������ ȣ�� (���� ������ ������ ������ ����)
```java
public class CancleOrderService {
    @Transactional
    public void cancleOrder(String orderId) {
        Order order = findOrderById(orederId);
        if (order == null) throw new OrderNotFoundException(orderId);
        order.cancel();
    }
}
```
  - ��ó�� ���� ���񽺴� ������ ���� �����ϱ⺸�� ������ �𵨿� ���� ������ �����Ѵ�.
  - Order��� �����ο� cancle\(\)�� ���Ͽ� �ֹ��� ����ϰ� �ִ�.

- ������
  - ������ ���� �����Ѵ�. (��ƼƼ, ��� ��)
  - ������ ���Ҵ� Order.class�� ���� ������ ���� �����Ѵ�.
  - �������� �ٽ� ������ �����Ѵ�.

- ������Ʈ��ó
  - ���� ����� ���� ���� �ٷ��.
  - **���� ���� �ߴ� ��ó�� ������ ����, ���� ����, ǥ�� ������ ���� ����� ����� �ڵ带 ���� ������ �ʴ´�.** 
  - RDBMS ����, �޽��� ť ����, ���� �߼�, Ÿ �ý��� REST API ȣ�� ���� ���� ������ �ٷ��.
  - �������� �������� ������ ǥ���ϱ� ���� `���� ����`�� �ٷ��.

ǥ��, ����, �������� ���� ���� ����� ����ϴ� �ڵ带 �ۼ����� �ʰ�, ������Ʈ��ó �������� �����ϴ� ����� ����Ѵ�.

## ���� ���� ��Ű��ó
�Ϲ������� ǥ�� -> ���� -> ������ -> ������Ʈ��ó ������ �� ������ ���� ������ �������� �ʴ´�. ������ ������ �������� ���� `���� -> ������, ������Ʈ����`, `������ -> ������Ʈ��ó` ������ ���� ������ ������Ʈ��ó ������ ��⿡ �����ϱ⵵ �Ѵ�.

### ���� ������ ������Ʈ��ó ������ ������ �� ������ִ� ������
���� ��� ���� �ݾ� ��� ����� �����ϴ� ���� ���񽺰� �ִٰ� �غ���.
������ ���� ��Ģ ����ϱ� ���� Drools��� �� ������ ����� ������Ʈ��ó�� ���� �����ϰ�, ���� ���񽺿��� �̸� ����Ѵ�. �� ���� ��, **ǥ��, ����, ������ ������ ���� ���� ����� �ٷ�� ������Ʈ���Ŀ� ������ �� �ִٴ� ���̴�.**
���� ��� �Ʒ�ó�� �� �� �ִ�.
Drools �� ���� ���� ���� (������Ʈ��ó)
```java
public class DroolsRuleEngine {
    private KieContainer kContainer;

    public DroolsRuleEngine {
        KieServices ks = KieServices.Factory.get();
        kContainer = ks.getKieClasspathContainer();
    }

    public void evalute(String sessionName, List<?> facts) {
        KieSession kSession = kContainer.newKieSession(sessionName);
        try {
            facts.forEach(x -> kSession.insert(x));
            kSession.fireAllrules();
        } finally {
            kSession.dispose();
        }
    }
}
```
���� �ݾ� ��� ���� (����)
```java
public class CalculateDiscountService {
    private DroolsRuleEngine ruleEngine;

    public CalculateDiscountService() {
        ruleEngine = new DroolsRuleEngine();
    }

    public Money calculateDiscount(List<OrderLine> orderLines, String customerId) {
        Customer customer = findCustomer(customerId);

        MutableMoney money = new MutableMoney(0);
        List<?> facts = Arrays.asList(customer, money); // Drools Ưȭ�ڵ� (���� ����� ���� ����)
        facts.addAll(orderLines); // Drools Ưȭ�ڵ� (���� ����� ���� ����)
        ruleEngine.evalute("discountCalculation", facts); // Drools Ưȭ�ڵ� (���� ����� ���� ����)
        return money.toImmutableMoney();
    }
}
```
�� �ڵ忡�� �� �� �ִ� �������� ������ ����.
> 1. ���� ������ ���������δ� �׽�Ʈ�� �Ұ����ϴ�. ������Ʈ��ó ����� ���� �۵� �ؾ��Ѵ�. ��, roolsRuleEngine�� �Ϻ��ϰ� �����ؾ߸� CalculateDiscountService�� �׽�Ʈ�� �� �ִ�.
> 2. ���� ��� ����� ��� Ȯ���� ����� ����. ���� ������ ������Ʈ��ó ����� �����ϸ� ���� ���� ���� ������ �����ؾ��Ѵ�.

������ ����� ����� �����Ϸ��� ������ ����� ����ؾ� �ϴµ�, ������Ʈ������ ��� Ư�� ����� ���� �����ϹǷ� �̷��� �������� �߻��ϰ� �ȴ�.
�̸� ��� ó���� �� ������?

## DIP
������ ������ ���� ������ �𵨿� �����ϵ��� �ٲٴ� ���̰� �̸� DIP(Dependency Inversion Principle, ���� ���� ��Ģ) �̶�� �Ѵ�.
�ٽ� �ѹ� CalculateDiscountService�� ���캸��, discount�� ��µ� � ������ ����ߴ��Ĵ� �߿����� �ʴ�.
����, ���������� ���������� ���� �����ؼ� ���� �ݾ��� ���Ѵٴ� ���� �߿��� ���̴�.
�� �κ��� �߻�ȭ�ؼ� �������̽��� ���� �� �ִ�.
```java
public interface RuleDiscounter {
    public Money applyRules(Customer customer, List<OrderLine> orderLines);
}
```
�� �������̽��� ����Ͽ� CalculateDiscountService���� DroolsRuleEngine�� ������ �� �ִ�.
```java
public class CalculateDiscountService {
    private RuleDiscounter ruleDiscounter;

    public CalculateDiscountService(RuleDiscounter ruleDiscounter) {
        this.ruleDiscounter = ruleDiscounter;
    }

    public Money calculateDiscount(List<OrderLine> orderLines, String customerId) {
        Customer customer = findCustomer(customerId);
        return ruleDiscounter.applyRules(customer, orderLines);
    }
}
```
Drools�� ����ϴ� RuleDiscounter ����
```java
public class DroolsRuleDiscounter implements RuleDiscounter {
    @Override
    public Money applyRules(Customer customer, List<OrderLine> orderLines) {
        // ���� ����
        ... 
    }
}
```
`CalculateDiscountService`�� �� �̻� ��������� `Drools(������)`�� �������� �ʰ�,
`���� �̿��� ���� �ݾ� ���` �� ǥ���ϴ� `RuleDiscounter` �������̽�(������)�� �����Ѵ�.
�׸��� �������̽��� ������ ������ ����� �ܺο��� ������ ����(Dependency Injection) ���ְ� �ȴ�.
�̿� ���� DIP�� ���������ν� ������ ������ ��⿡�� ������ ��� ��뿡�� ���� ���������� �ذ��� �� �ְ� �ȴ�.

#### DIP ���ǻ���
- �ܼ��� �������̽��� ���� Ŭ������ �и��ϴ� ���� DIP�� �ƴϴ�.
- �ٽ��� ������ ����� ������ ��⿡ �������� �ʵ��� �ϴ°��̴�.
- �ùٸ� DIP�� ���� �������̽� ���� �� **������ ����(������)**���� �ٶ󺸴� ���� �߿��ϴ�.


## ������ ������ �ֿ� �������
- ��ƼƼ (ENTITY)
  - ������ �ĺ��ڸ� ������ �ڽ��� ������ ����Ŭ�� ���� ��ü
  - �������� ������ ������ ǥ��. �����Ϳ� ���� ����� �����Ѵ�. 
- ��� (VALUE)
  - ������ �ĺ��ڸ� ���� �ʰ� �ַ� ��ƼƼ�� �Ӽ��� ǥ���� �� ���Ǵ� ��ü
  - �ٸ� ��� Ÿ���� �Ӽ����ε� ���� �� �ִ�.
- �ֱ׸���Ʈ (AGGREGATE)
  - ���� ��ƼƼ�� ��� ��ü�� ���������� �ϳ��� ���� ��. ������� `�ֹ�` �������� �ֹ� ��ƼƼ, �ֹ� �׸� ��ƼƼ, �ֹ��� ��ƼƼ�� `�ֹ�` �ֱ׸���Ʈ�� �������ִ�.
- �������͸� (REPOSITORY)
  - ������ ���� ���Ӽ��� ó��. ��ǥ������ DBMS�� ����, ��ȸ�ϴ� ������ ��� 
- ������ ���� (DOMAIN SERVICE)
  - Ư�� ��ƼƼ�� ������ ���� ������ ������ ����. ������� `���� �ݾ� ���` ����� ��ǰ, ����, ȸ�� ��� ���� �ݾ� �� ���� ��ƼƼ�� ����� �ʿ�� �ϴµ� �̷� �� ������ ���񽺸� ���� ����� ����.

### ��ƼƼ�� ���
������ ���� ��ƼƼ�� DB ���� ��ƼƼ�� �������� �ִ�.
- DB ��ƼƼ�� �ش� ������ �����͸� ǥ���ϴ� �ݸ� ������ ���� ��ƼƼ�� ������ �Ӹ� �ƴ϶� ***������ ����� �Բ� �����Ѵ�.***
- DB ��ƼƼ�� �Ӽ��� �÷����� �ۿ� ǥ���� �� ���µ� ������ ���� ��ƼƼ�� ��� Ÿ���� ����� �� �ִ�.
  - ���� DB������ ���� �����͸� ���� ���� ���̺��� �и��� �� ������, ��� Ÿ���� �ǹ̰� �巯���� ����
  - ��� Ÿ�Կ��� `����`�̶� ��� ��ü ��ü�� ��ü�ϴ°ǵ�, ���̺� �и���� �� ������ ���̰� �̴� ��Ʃ Ÿ�� ������ ����Ǵ� ����� `�Һ�`�� �����

##### �����ͺ��̽� ��ƼƼ
> ����Ÿ���̽� �о߿��� ��ü �Ǵ� ��ƼƼ(Entity)��� �ϴ� ���� ����Ÿ���̽��� ǥ���Ϸ��� �ϴ� ����, ������ ��ü�μ� ���� �����Ǵ� ���� ���Ѵ�. �� ��ü�� ���� ���迡 ���� ����� �����ϴ� �����̳� ������ �����μ� �ǹ̸� ������ �ִ�. �̰��� ��ǻ�Ͱ� ����ϴ� ȭ���� ���ڵ�(record)�� �����Ѵ�. �� ��ü�� �� �ܵ����� ������ �� ������, �����μ��� ������ �Ѵ�. �ϳ��� ��ü�� �ϳ� �̻��� �Ӽ�, �� ��Ʈ����Ʈ(attribute)�� �����ǰ� �� �Ӽ��� �� ��ü�� Ư���̳� ���¸� ����� �ش�.

### �ֱ׸���Ʈ
�������� Ŀ������ ������ ������ �𵨵� Ŀ���� �ǰ�, ���� ��ƼƼ�� ������ ����鼭 ���� ���� �� ����������. �̷��� ������ ���� ���������� �����ڰ� ��ü ������ �ƴ� �Ѱ� ��ƼƼ�� ������ �����ϰ� �Ǵ� ��찡 �߻��Ѵ�.

������ �� �� �ſ� ���ϰ� ���� ������ ������ ���� ū ���ؿ��� ��� ��ġ�ϰ� �ִ��� �����ϱ� �����Ƿ� ū ���ؿ��� �����ִ� ������ ������ �Բ� ���� ���� ��ġ�� ���� �� ��Ȯ�ϰ� ������ �� �ִ�.
�̿� ����ϰ� ������ �𵨵� ���� ��ü�� �ƴ϶� ���� ���ؿ��� ���� �� �� �־�� ��ü �𵨰� ���� ���� �����ϴµ� ������ �ȴ�.
�̰� �ٷ� �ֱ׸���Ʈ(AGGREGATE) �̴�.
- �ֱ׸���Ʈ�� ���� ��ü�� �ϳ��� ���� ������ ���� ���� ���ؿ��� �����ϴ� ���� ���´�.
  - `�ֹ�` �ֱ׸���Ʈ�� `�ֹ�`,`�ֹ���`,`�������` ���� �����Ѵ�.
- �ֱ׸���Ʈ�� �ش� ������ ���� ��ü���� �����ϴ� ��Ʈ ��ƼƼ�� ���´�. (�ֱ׸���Ʈ ��Ʈ)
  - ��Ʈ ��ƼƼ���� �ش� �ֱ׸���Ʈ�� ���� �ִ� ��ƼƼ�� ��� ��ü�� �̿��� �ֱ׸���Ʈ�� �����ؾ��� �������� ����� ���� (ĸ��ȭ)
  - �ֱ׸���Ʈ ��� �������� �ֱ׸���Ʈ ��Ʈ�� ���ؼ��� �ش� �ֱ׸�ĿƮ �� �ٸ� ��� ��ü�� ���� (å 60P)

> OOP���� ��ü�� ĸ��ȭ�� �ֱ׸���Ʈ�� ������ �����غ��� �� �� ���ذ� ����.
>* ��ü�� �ʵ�(�Ӽ�), �޼ҵ带 �ϳ��� ����, ���� ���� ������ �ܺο� ���ߴ� ���� ���Ѵ�. => �ֱ׸���Ʈ ��Ʈ�̿� ���� ��ƼƼ �� ����� �������� ����
>* �ܺ� ��ü�� ��ü ������ ������ ���� ���ϸ� ��ü�� �����ؼ� �����ϴ� �ʵ�� �޼ҵ常 �̿��� �� �ִ�. => �ֱ׸���Ʈ ��Ʈ�� ���ؼ��� ������ ��� ȣ�� ����
>* �ʵ�� �޼ҵ带 ĸ��ȭ�Ͽ� ��ȣ�ϴ� ������ �ܺ��� �߸��� ������� ���� ��ü�� �ջ���� �ʵ��� �ϴµ� �ִ�. => �ֱ׸���Ʈ�� ���� ������ �ǵ��� ��������θ� �����ϰ� ��

### �������͸�
������ ���� ���������� ����Ϸ��� RDBMS, NoSQL, ���� �� ������ ����ҿ� ����(����ȭ)�ؾ��Ѵ�. �̸� ���� ������ ���� �������͸��̴�. 
- �������丮�� �ֱ׸���Ʈ ������ ������ ��ü�� ��ȸ�ϰ� �����ϴ� ����� �����Ѵ�.
- �������͸��� ������ ��ü�� ����ȭ�ϴµ� �ʿ��� ����� �߻�ȭ �� ���̱� ������ ������ ��⿡ ���ϰ�, ���� ���� Ŭ������ ������Ʈ���� ������ ���Ѵ�.

- ��ƼƼ�� ����� �䱸���׿��� ����Ǵ� ���̶��, �������͸��� �����ܰ迡�� ����Ǵ� ��
- �ֹ� �ֱ׸���Ʈ�� ���� �������͸� ���� 
```java
public inferface OrderRepository {
    public Order findByNumber(OrderNumber number);
    public void save(Order order);
    public void delete(Order order);
}
```
- �� ���ÿ��� �������͸����� �����ϴ� ������ ���� ������ �ֹ� �ֱ׸���Ʈ ��Ʈ�� �ֹ�(Order)�̴�.
- ��������� �ֹ� ��ƼƼ�� �ֹ� �ֱ׸���Ʈ�� ���� ���� ������ �𵨵��� ���� �����ϰ� �ֱ� ������ �ֹ� �ֱ׸���Ʈ ���� ó���� �ȴ�.
- �ֹ� ��� ����� �����ϴ� ���� ���� ����
```java
public class CancelOrderService {
    private OrderRepository orderRepository;

    public void cancel(OrderNumber number) {
        // �������͸��� ���� ������ ��ü ��ȸ
        Order order = orderRepository.findByNumber(number);
        if (order == null) throw new NoOrderException(number);

        // ������ ��ü�� ������ ��� ��� ����
        order.cancel();
    }
}
```
- ������ �� �������� OrderRepository�� ������ ��ü�� ����ȭ �ϴµ� �ʿ��� ����� interface�� �߻�ȭ�� ������ ������ ��⿡ ���� (å 63p)
- ������ JPA ������ OrderRepositofy�� ����(implement)�ϴ� Ŭ������ ������ ���� ������ ������ ���Ѵ�.
- ������ �����ӿ�ũ�� ����Ѵٸ� �Ʒ��� ���� ���� ���񽺸� ������ �� �ִ�.
```java 
@Configuration
public class OrderServiceConfig {
    @Autowired
    private OrderRepository orderRepository;

    @Bean
    public CancelOrderService cancelOrderService() {
        return new CancelOrderService(orderRepository);
    }
}
```
```java
@Configuration
public class RepositoryConfig {
    @Bean
    public JpaOrderRepository orderRepository() {
        return new JpaOrderRepository();
    }

    @Bean
    public LocalContainerEntityManagerFactoryBean emf() {
        ...
    }
}
```
- ���� ���񽺿� �������͸��� ������ ���� ������ ������ ������ �ִ�.
  - ���� ���񽺴� �ʿ��� ������ ��ü�� ���ϰų� �����ϱ� ���� �������͸��� ���
  - ���� ���񽺴� Ʈ������� �����ϴµ�, Ʈ����� ó���� �������͸� ���� ����� ������ ����
- �������͸��� ��� ��ü�� ���� �����̱� ������ �������͸��� ���� ���񽺰� �ʿ�� �ϴ� �޼��带 ����
  - �ֱ׸���Ʈ�� �����ϴ� �޼��� `void save(Aggregate agg)`
  - �ֱ׸���Ʈ�� �����ϴ� �޼��� `Aggregate findById(SomeId id)`
  - �ʿ信 ���� `delete`, `counts` �� �߰� ��� ����

## ��û ó�� �帧
![](../../assets/��ûó���帧.jpeg)
1. ����ڰ� ���ø����̼ǿ� ��� ���� ��û�� ǥ�� �������� �޴´�.
2. ������� ��û�� �˻��ϰ� ���� ���񽺰� �䱸�ϴ� �������� ��ȯ�Ͽ� ���� �������� �����Ѵ�.
3. ��� ������ �ʿ��� ������ ��ü�� Repository���� ������ ������ ������ �����ϰų� �ű� ������ ��ü�� �����ؼ� Repository�� �����Ѵ�.
    - ������ ��ü�� ����ؼ� ������� ��û�� ó��
    - Ʈ����� ���� �� ó�� ex) ȸ������ ���� ��, ȸ�� ������ DB�� �ݿ��ؾ���.
    - @Transactional
4. ���� ����� ����ڿ��� �����Ѵ�.

## ������Ʈ��ó ����
ǥ��, ����, ������ ������ �����ϴ� �����̴�.
  - ������Ʈ��ó�� ���� ����ϴ� �ͺ��� ������ �������� ������ �������̽��� ������Ʈ��ó �������� �����ϴ� ���� ����.
    - ��ü�� �׽�Ʈ�� ������� �ذ��� �� �ִ�.
  - ������ ������Ʈ��ó�� ���� ������ ���ִ� ���� ���� ���� �ƴϴ�. 
    - @Transactional�� ����ϴ� ���� �����ϴ�.
  - ������ ������ ���� �ʵ��� �õ��ϴ� ���� ������ �� �����ϰ� ��ư� ���� �� �ִ�.
����, DIP�� ������ ��ġ�� �ʴ� �������� ���� ������ ������ �������� ���� ����� ���� ������ �������� ���� �����ϴ�.

## ��� ����
- �� ��Ű���� �����ϸ� 10�� �̸����� Ÿ�� ������ �����Ϸ��� ����Ѵ�.
- �� ������ �Ѿ�� ����� �и��Ͽ� �õ��� ����.