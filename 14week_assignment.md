# 학습할 것 (필수)
- 제네릭 사용법
- 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
- 제네릭 메소드 만들기
- Erasure

# 제네릭 사용법

## 제네릭이란?
- JDK1.5 도입
- 제네릭(Generic)은 클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법
- 제네릭스는 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일시 타입체크를 해주는 기능


## 제네릭 사용 하는 이유
1. 컴파일 시 강한 타입체크를 할 수 있음
    - 실행시 타입 에러가 나는 것보다 컴파일 시 미리 타입을 강하게 체크해서 에러를 사전에 방지
2. 타입 변환(casting)을 제거
    - 비제네릭 코드는 불필요한 타입 변환을 하기 때문에 프로그램 성능에 악영향을 미침


## 제네릭 사용법
- 제네릭 타입은 타입을 파라미터로 가지는 클래스와 인터페이스를 말함
- 제네릭 타입은 클래스 또는 인터페이스 뒤에 <> 부호가 붙고 사이에 타입 파라미터가 위치
```java
public class 클래스명<T>{...}
public interface 인터페이스명<T>{...}
```

## 자주 사용하는 타입인자
|타입인자|설명|
|---|---|
|<T>|Type|
|<E>|Element|
|<K>|Key|
|<N>|Number|
|<V>|Value|
|<R>|Result|

# 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
## 바운디드 타입(Bounded type parameter)
- 특정 타입의 서브 타입으로 제한함
- 클래스나 인터페이스 설계시 흔하게 사용되는 개념
```java
    public class BoundTypeSample <T extends Number>{ // Number의 서브 타입만 허용 . Integer는 Number의 서브 타입
        public void set(T value){}

        public static void main(String[] args){
            BoundTypeSample<Integer> boundTypeSample = new BoundTypeSample<>();
            boundTypeSample.set("Hi"); // 컴파일 에러 - 인자로 문자열을 전달하였기 때문에
        }
    }
```

## 와일드 카드(WildCard)
- 제네렉으로 구현된 메소드의 경우 선언된 타입으로만 매개변수를 입력해야함
- 이를 상속받은 클래스 혹은 부모클래스를 사용하고 싶어도 불가능하고 어떤 타입이 와도 상관없는 경우에 대응하기 좋지 않다.
- 이를 위한 해법으로 WildCard를 사용
### 종류
- Unbounded WildCard
    - <?> 제한 없음
- Upper Bounded WildCard
    - <? extends T>
    - 와일드 카드의 상한 제한(upper bound) - T와 그 자손드을 구현한 객체들만 매개변수로 가능
- Lower Bounded WildCard
    - <? super T>
    - 와일드 카드의 하한 제한(lower bound) - T와 그 조상들을 구현한 객체들만 매개변수로 가능

# 제네릭 메소드 만들기
- 제네릭 메소드 :  매개타빙과 리턴타입 파라미터를 갖는 메소드
- 구현하기 위해서 리턴 타입 앞에  <> 다이아몬드 기호를 추가하고 , 타입 파라미터를 기술한 다음린턴 타입과 매개타입으로 타입 파라미터를 사용

```java
public <타입파라미터...> 리턴타입 메소드명(매개변수,...){...}

public <T> Box<T> boxing(T t){...}
```

## 제네릭 메소드 호출 방식
1. 리턴타입 변수 = <구체적타입> 메소드명(매개값); // 명시적으로 구체적 타입지정
2. 리턴타입 변수 = 메소드명(매개값); // 매개값을 보고 구체적 타입을 추정

일반적으로 매개값을 넣어줌으로 컴팡이러가 유추하게 만들어주는 2번째 방법 사용

```java
1. Box<Integer> box = <Integer>boxing(100); // 타입 파라미터를 명시적으로 Integer로 지정
2. Box<Integer> box = boxing(100); //타입 파라미터를 Integer로 추정
```


# Type Erasure
- 자바에서 제네릭 클래스를 인스턴스화 할때 해당 타입을 지워버림
- 그 타입은 컴파일 직전까지만 존재하고 컴파일된 바이트 코드에서는 어떠한 타입 파라미터도의 정보를 찾아 볼수 없음
```java
List<Integer> numbers = new ArrayList<>();
```
위 코드의 바이트 코드
```java
L0
LINENUMBER 11 L0
NEW java/util/ArrayList
DUP
INVOKESPECIAL java/util/ArrayList.<init> ()V
ASTORE 1
```
- 위와 같이 ArrayList를 생성 할때, 어떠한 타입 정보도 들고 있지 않음
- new ArrayList()로 생성 한 것과 동일하게 바이트 코드가 생성됨
- 하위 호환성, 즉 Java 제네릭으로 작성된 코드도 Java4에서 돌아 갈 수 있도록 제공 했지만, 프로그래머에세 혼란을 제공함



# 참고
- https://yaboong.github.io/java/2019/01/19/java-generics-1/
- https://brunch.co.kr/@kd4/146
- https://sujl95.tistory.com/73
- https://devlog-wjdrbs96.tistory.com/203
- https://dev-coco.tistory.com/28
