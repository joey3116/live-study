# 학습할 것 (필수)
- enum 정의하는 방법
- enum이 제공하는 메소드 (values()와 valueOf())
- java.lang.Enum
- EnumSet
---

# enum 정의하는 방법
## enum 이란?
> 열거 타입은 일정개수의 상수 값을 정의한 다음, 그외의 값은 허용하지 않는 타입이다.
- 서로 연관된 상수들의 집합
- JDK 1.5 부터
- enum 타입은 클래스이다.
- enum 상수 하나당 인스턴스가 만들어 지며 , 각각 `public static final`이다.


## 장점
- 열거체를 비교할 때 실제 값뿐만 아니라 타입까지도 체크
- 열거체의 상숫값기 재정의되더라도 다시 컴파일할 필요가 없다.

## enum 정의 하는 방법
- `enmu` 키워드를 사용하여 열거체 정의 할 수 있다.
```java
public enum Fruits{
    APPLE,
    ORANGE,
    BANANA, 
    PEAR
}
```

# enum이 제공하는 메소드 (values()와 valueOf())
## values()
- 해당 열거체의 모든 상수를 저장한 배열을 생성하여 반환
- 자바의 모든 열거체에 컴파일러가 자동으로 추가해 주는 메소드

## valueOf()
- 전달된 문자열과 일치하는 해당 열거체의 상수를 반환

## ordinal() - 안티패턴
- 해당 상수가 몇번째 인지 리턴
- EnumSet과 EnumMap 같이 열거타입 기반의 범용 자료구조에 쓸 목적으로 설계됨

# java.lang.Enum
- 모든 enum들은 내부적으로 java.lang.Enum 클래스에 의해 상속된다. 따라서, enum은 다른 것을 상속 할 수 없다.
- java.lang.Enum 클래스에는 열거체를 조작하기 위한 다양한 메소드가 포함되어있다.


# EnumSet
- `enum` 타입에 사용하기 위한 특수한 `Set` 구현
- 다른 컬렉션들과 다르게 new 연산자 사용이 불가능
- 구현 객체 반환 받을 쌔 사용하는 메소드 : `noneof`

```java
public abstract class EnumSet { //EnumSet은 abstract하여 객체 생성이 불가능하다.
    ...
    
    //noneOf 메소드에서 상황에 따라 다른 구현체 객체들을 만들어서 반환해주고 있다.
    public static noneOf(...) {
        if (enumElementSize <= 64)
            return new RegularEnumSet<>(...);  
        else
            return new JumboEnumSet<>(...);
    }
}
```

```java
    EnumSet<Fruits> of = EnumSet.of(Fruits.APPLE, Fruits.ORANGE);
    EnumSet<Fruits> all = EnumSet.allOf(Fruits.class);
    EnumSet<Fruits> none = EnumSet.noneOf(Fruits.class);
    EnumSet<Fruits> inner = EnumSet.range(Fruits.APPLE, Fruits.ORANGE);
```

# 참조
- https://velog.io/@pop8682/Enum-27k067ns4a
- http://www.tcpschool.com/java/java_api_enum
- https://siyoon210.tistory.com/152
- https://johngrib.github.io/wiki/java-enum/