# 학습할 것 (필수)
- 인터페이스 정의하는 방법
- 인터페이스 구현하는 방법
- 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스 상속
- 인터페이스의 기본 메소드 (Default Method), 자바 8
- 인터페이스의 static 메소드, 자바 8
- 인터페이스의 private 메소드, 자바 9
---

# 인터페이스 정의 하는 방법
## 인터페이스
> 클래스들이 구현해야 하는 동작을 지정하는데 사용되는 추상 자료형

자바에서 인터페이스의 역할
- 개발자 사이의 코드 규약 정함
- 여러 구현체에서 공통적인 부분을 추상화한다.(다형성)
---
- 정의
```java
    public interface Shape{
        void draw();
    }
```

- 멤버는 추상 메소드와 상수만으로 구성된다.
    - Java 8 까지 : 추상메소드만 가능
    - Java 8 부터 : default 메소드, static 메소드가 추가되었다.
    - 추상메서드 : 구현부가 없는 메소드    

    - 구현 클래스는 인터페이스에 명시된 추상메서드들을 모두 구현해야함
    ```java
        public class Circle implements Shape {
            // ...
            @Override
            public void draw() {
             // ...
            }
        }
    ```
- 모든 메소드는 public 이며 생략이 가능하다.
- 상수도 public static final을 생략 하여 선언 할 수 있다.
    - 필드 변수를 선언하면 `public static final` 로 선언해야 하지만 생략 가능
- 인터페이스의 객체를 생성 할수 없다.
    - 생성자를 가질 수 없다.
- 다른 인터페이스에 상속될 수 있다.
- 인터페이스도 레퍼런스 변수의 타입으로 사용가능하다.

## 인터페이스와 추상클래스
|비교 |내용 |
|----|----|
|추상클래스|일반메소드 포함가능. 상수,변수 필드 포함가능. 모든 서브 클래스에 공통된 메소드가 있는 경우에는 추상클래스가 적합
|인터페이스|상수필드만 포함가능, 다중 상속 지원|



# 인터페이스 구현하는 방법
### 구현
- 인터페이스의 추상메서드를 클래스에서 구현하는 것을 말한다.
- `implements` 키워드를 사용하여 구현
- 이떄 클래스는 반드시 인터페이스의 모든 추상 메소드를 구현해야 한다. 
- 여러개의 인터페이스를 구현할떄는 콤마로 인터페이스를 구분하여 나열하고, 각 인터페이스의 정의된 모든 추상 메소드를 구현해야 한다.
```java
    interface USBMouseInterface{
        void mouseMove();
        void mouseClick();
    }
    
    interface RollMouseInterface{
        void roll();
    }
    public class MouseDrice implements USBMouseInterface, RollMouseInterface{
    @Override
    public void mouseMove() {
        // 구현...
    }

    @Override
    public void mouseClick() {
        // 구현...
    }

    @Override
    public void roll(){
        // 구현...
    }
}
```


# 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
- 인터페이스에 의한 다형성
- 인터페이스 역시 클래스와 마찬가지로 변수 타입으로 사용 될 수 있다.
- 인터페이스를 참조 변수로 사용 시 , 그 참조 변수가 가리키고 있는 객체에만 있는 메소드는 호출 할 수 없고, 인터페이스에서 정의된 메소드만 호출 가능
```java
public class MainClass{
    static void main(String[] args) {
        Shape shape = new Circle();
        shape.draw();
        shape.getRadius(); // 에러 
    }
}

class Circle implements Shape{
    int radius;
    @Override
    public void draw() {
        System.out.println("draw circle");
    }
    public int getRadius(){
        return radius;
    }
}
interface Shape {
    void draw();
}
```


# 인터페이스 상속
인터페이스는 구현과 상속을 모두 할 수 있다.
- 인터페이스를 사용하는 구현 클래스는 해당 인터페이스를 구현해야한다.
- 인터페이스 사이에는 상속을 할 수 있다.
- 다중 상속이 가능하다.
```java
    interface MobilePhone{
        public boolean sendCall();
        public boolean receiveCall();
    }
    class PDA{
        public int calcuate(int x, int y){
            return x + y;
        }
    }
    public class SmartPhone extends PDA implements MopbilePhone{
        @Override
        public boolean sendCall(){
            // 구현...
        }
        @Override
        public boolean receiveCall(){
            // 구현...
        }
    }
```

# 인터페이스의 기본 메소드 (Default Method), 자바 8
- Java8 이상 부터는 `default` 접근 제어자 사용 할 수 있게 되었다.
- 인터페이스 내에서 직접 메서드를 구현한다는 의미
- 구현한 클래스는 오버라이딩 없이 `default` 메소드 사용하거나, 오버라이딩하여 재정의 할 수 있다.


# 인터페이스의 static 메소드, 자바 8
- Java8에서 인터페이스에 static 으로 메서드를 구현할 수 있도록 변경되었다.
- 재정의는 불가능
- 인스턴스 생성하지 않고 인터페이스 메서드를 사용할 수 있음

# 인터페이스의 private 메소드, 자바 9
클래스 내부에서만 접근할 수 있으며 내부 인터페이스를 구성 할때 쓰임


# 출처
- 명품 Java programming
- https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4_(%EC%9E%90%EB%B0%94)
- https://velog.io/@codemcd/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4Interface
- http://hong.adfeel.info/backend/java5/
- https://jinbroing.tistory.com/215
- https://dzone.com/articles/learning-java-what-vs-why
