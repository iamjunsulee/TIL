# Builder Pattern
정적 팩토리 메서드와 생성자 모두 매개변수가 많을 경우 적절히 대처하기 어렵다. 

### 점층적 생성자 패턴
점층적 생성자 패턴이란 필수 매개변수만 받는 생성자, 필수 매개변수와 선택 매개변수 1개를 받는 생성자, 선택 매개변수를 2개까지 받는 생성자, ..형태로 선택 매개변수를 전부 다 받는 생성자까지 늘려가는 방식이다.

```java
public class Person {
    private String name;
    private int age;
    private String phone;
    
    public Person() { }
    
    public Person(String name) {
        this.name = name;
    }
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name, int age, String phone) {
        this.name = name;
        this.age = age;
        this.phone = phone;
    }
}
```
위 코드를 보면 문제가 없어보인다. 하지만 멤버 변수가 3개가 아니라 수십 개라고 생각해보자. 일일히 다 만들어주는 것도 번거롭지만 내가 원치않는 매개변수에 값을 할당해야하는 경우도 생길지도 모른다.
매개변수가 많아지면 많아질수록 코드도 점점 복잡해질 것이다.

### 자바 빈즈 패턴(Java Beans Pattern)
매개변수 없는 생성자로 객체를 만든 후, Setter 메서드를 통해서 원하는 매개변수 값을 설정하는 방식이다.

```java
@Setter
public class Person {
    private String name;
    private int age;
    private String phone;

    public Person() { }
}
```
```text
Person person = new Person();
person.setName("leejunsu");
person.setAge(32);
person.setPhone("01012341234");
```
점층적 생성자 패턴에 비해서 인스턴스를 만들기 쉽고, 알아보기 쉽다.
_하지만 객체 하나를 생성하기 위해서 Setter 메서드를 여러 개 호출해야 하고, 객체가 완전히 완성되기 전까진 Setter 메서드를 통해 객체가 변할 수 있기 때문에 일관성 유지가 어렵다._

### 빌더 패턴
