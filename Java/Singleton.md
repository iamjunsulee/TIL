# private 생성자나 열거 타입으로 싱글턴임을 보증하라

### 싱글턴이란
인스턴스를 오직 하나만 생성할 수 있는 클래스
하나의 인스턴스만 생성하므로 모든 클라이언트에게 동일한 인스턴스를 반환하게 된다.

### 싱글턴을 만드는 방법
인스턴스를 생성함에 있어 보통 new 메서드를 많이 사용한다. 그렇다면 인스턴스를 오직 하나만 생성하려면 어떻게 해야할까?  
**인스턴스를 생성에 제약을 걸어야한다. 생성자를 외부에서 사용하지 못하도록 private 접근 제어자를 지정하고, 유일한 인스턴스를 반환하도록 정적 메서드를 제공해야 한다.**
```java
public class Singleton {
    private static final Singleton instance = new Singleton();
    private Singleton() { }
    public static Singleton getInstance() {
        return instance;
    }
}
```