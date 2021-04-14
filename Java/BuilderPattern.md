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
