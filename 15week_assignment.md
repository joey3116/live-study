# 학습할 것 (필수)
- 람다식 사용법
- 함수형 인터페이스
- Variable Capture
- 메소드, 생성자 레퍼런스

# 람다식
- 익명함수(Anonymous functions)를 지칭하는 용어
- 메소드를 하나의 간결한 식(expression)으로 표현한 것
- 익명객체를 생성하기 위한 표현식
- Java8부터 지원


## 람다식 사용법
매개변수 + 실행문으로 구성됨
- 즉 접근자, 반환형 모두 생략되는 구조
- 모양 ()->{}; 형태
- () 는 interface 함수의 매개변수를 입력
- {} 안에 실행할 코드를 작성

기존 java
```java
public class LamdaExample{
    public static void main(String[] args){
        Thread thread = new Thread(new Runnable(){
            @Override
            public void run(){
                System.out.println(Thread.currentThread().getName());
            }
        });

        thread.start();
    }
}
```
람다식 사용
```java
public class LamdaExaple{
    public static void main(String[] args){
        Thread thread = new Thread(()-> System.out.println(Thread.currentThread().getName());
        thread.start();
    }
}
```

```java
// 함수형 인터페이스
public interface Calculator{
    public int cal(int num1, int num2);
}
```
아래의 람다식에서 사용될 인터페이스이다.

1. 기본 사용법 
- (매개변수 타입)->{};
- 람다식에 필요한 기호를 모두 사용한 방법
```java
public static void main(String[] args){
    Calculator cal = (int num1, int num2)-> {return num1 + num2};
    System.out.println(cal.cal(1,2));
}
```

2. 매개변수 타입생략
- (매개변수)->{};
- 매개변수가 1개이거나, 2개 이사으이 매개변수의 타입이 모두 같을 떄에는 타입을 생략
```java
    public static void main(String[] args){
        Calculator cal = (num1, num2) -> {return num1 + num2;};
        System.out.println(cal.cal(1,2));
    }
```
3. 매개변수가 없는 경우 
- ()->{};
- 만약 Calculator 인터페이스의 cal() 메소드에 매개변수가 없다면 아래와 같이 매개변수를 생략하여 작성가능
```java
    public static void main(String[] args){
        Calcaltor cal = () -> {System.out.println("매개변수가 없는 경우")};
        cal.cal();
    }
```

4. 중괄호 생략 
- ()->;
- 실행할 문장이 1개일떄는 {} 를 생략 가능
- 이때 , 반환이 필요한 메소드의 경우 return 키워드를 생략해야 한다는 것
```java
    public static void main(String[] args){
        Calculator cal = (num1 , num2)-> num1 + num2;
        System.out.println(cal.cal(1,2));
    }
```

5. 소괄호 중괄호 생략
- 매개변수 ->;
- 매개변수가 1개이고 실행할 문장도 1개이면 아래와 같아 생략 가능
```java
    public static void main(String[] args){
        Calculator cal = num1 -> System.out.println(num1);
        cal.cal(1);
    }
```

## 람다식의 장단점
### 장점
- 코드를 간결하게 만듬
- 코드가 간결하고 식에 개발자의 의도가 명확히 드러나므로 가독성이 향상
- 함수를 만드는 과정이 없이 한번에 처리할 수 있기에 코딩하는 시간이 줄어듬
- 병령프로그래밍이 용이

### 단점
- 람다를 사용하면서 만드는 무명함수는 재사용이 불가
- 디버깅이 다소 까다로움
- 람다를 남발하면 코드가 지저분해질 수 있음(비슷한 함수를 계속 중복생성할 가능성이 높음)
- 재귀로 만들경우에는 다수 부적합한면이 있음

## 람다 사용 조건
- 구현해야할 인터페이스의 추상 메소드가 단 1개 이어야 함

# 함수형 인터페이스(Functional Interface)
- 람다식을 다루는 인터페이스
- 람다 표현식을 사용 할떄는 람다 표현식을 저장하기 위한 참조 변수의 타입을 결정해야함
- 참조변수의타입 참조변수의이름 = 람다표현식
- 람다 표현식을 하나의 변수에 대입할 때 사용하는 참조 변수의 타입을 함수형 인터페이스라 부름
- @FunctionalInterface 어노테이션을 사용
- @FunctionalInterface가 적용된 인터페이스는 한개의 추상메소드만 선언 가능
- 추상메소드가 1개가 아니면 에러 발생
```java
    @FunctionalInterface
    interface Calc{
        public int min(int x, int y);
    }
```

## java.util.function 패키지
- 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 정의해 놓음

|함수형 인터페이스|메서드|설명|
|---|---|---|
|java.lang.Runnable| void run()|매개변수도 없고, 반환값도 없음|
|Supplier| T get() | 매개변수는 없고, 반환값만 있음|
|Consumer| void accept(T t) | Supplier와 반대로 매개변수만 있고, 반환값이 없음|
|Function | R apply(T t)| 일반적인 함수. 하나의 매개변수를 받아서 결과를 반환|
|Predicate| boolean test(T t) | 조건식을 표현하는데 사용. 매개변수는 하나, 반환 타입은 boolean|
|BiConsumer| void accept(T t, U u)|두개의 매개변수만 있고 반환값이 없음|
|Bipredicate| void test(T t, U u)|조건식을 표현하는데 사용됨, 매개변수는 둘, 반환값은 boolean|
|BiFunction| R apply(T t, U u)|두개의 매개변수를 받아서 하나의 결과흘 반환

- 수학에서 결과로 true 또는 false를 반환하는 함수를 Predicate 라고 함
- 매개변수가 2개인 함수형 인터페이스는 이름 앞에 `Bi`ㄱㅏ 붙음
- Supplier는 매개변수는 없고 반환값만 존재하는데 메서드는 두개의 값을 반환할 수 없으므로 BiSupplierㄱㅏ 없음
- 매개변수의 타입과 반환타입이 일치 할떄는 Function 대신 UnarayOperator를 사용(매개변수 2개면 BinaryOperator)

# Variable Capture
람다의 바디에서는 파라미터 말고 바디 외부에 있는 변수를 참조 가능
```java
public class LamdaCapturing{
    private int a =12;

    public void test(){
        int b = 123;
        final Runnable r = () -> System.out.println(a);
        final Runnable r2 = () -> System.out.println(b);
    }
}
```
- 위와 같이 파라미터로 넘겨진 변수가 아닌, 외부에서 정의된 변수를 자유 변수(Free Varaiable)이라 함
- 람다 바디에서 자유변수를 참조하는 행위 : 람다 캡처링(Lamda Capturing)

## 람다 켭처링의 제약 조건
1. 지역 변수는 final로 선언되어있어야 함
2. final로 선언되지 않은 지역변수는 final 처럼 동작해야함
    - 즉, 값의 재할당이 일어나면 안됨
```java
public class LamdaCapturing{
    private int a = 12;
    
    public void test(){
        final int b = 123;
        int c = 123;
        int d = 123;
        
        final Runnable r = ()->{
            // 인스턴스 변수 a는 final로 선언되 있을 필요도, final처럼 재할당하면 안된다는 제약조건도 적용되지 않음
            a = 123;
            System.out.println(a);
        };
        // 지역변수 b는 final로 선언돼 있음 (O)
        final Runnable r2 = () -> System.out.println(b);

        // 지역변수 c는 final로 선언되 있지 않지만 final을 선언한 것과 같이 변수에 값을 재할당하지 않았으므로 (O)
        final Runnable r3 = () -> System.out.printl(c);

        // 지역 변수 d는 final로 선언되지 않고, 값의 재할당 하였으므로 final처럼 동작하지 않기에 (X)
        d = 12;
        final Runnable r4 = () -> System.out.println(d);
    }
}
```
##  이유
JVM 메모리 구조를 알아야함
- 지역변수는 스택 영역에 생성
- 인스턴스변수는 힙 영역에 생성
- 스택영역은 쓰레드마다 별도의 스택이 생성
    - 따라서 지역벽수는 쓰레드끼리 공유가 안됨
- 인스턴스 변수는 쓰레드끼리 공유 가능

- 람다는 별도의 쓰레드에서 실행이 가능
- 원래 지역 변수가 있는 쓰레드는 사라져서 해당 지역변수가 사라짐
- 하지만 이 람다에서 사라진 쓰레드의 지역변수를 참조하고 있으면 오류가 나야된다. 
- 하지만 오류가 나지 않는다. 
- 또한 별도의 쓰레드에서 실행된다면 별도의 스택영역을 가지고, 다른 쓰레드의 스택에 있는 지역변수는 참조조차 할 수 없다. 
- 하지만 오류 없이 참조가 가능 하다. 
- 이는 람다에서 지역변수(해당 쓰레드의 스택)에 직접적으로 접근하는게 아니라, 변수를 자신(쓰레드)의 스택에 복사하기 때문임
- 따라서 쓰레드의 스택에 있는 지역변수와 동일한 값을 참조할 수 있고, 원래 쓰레드가 사라져도 본인의 쓰레드에서 자신의 할일을 할 수 있음
- 하지만 이와 같이 변수를 복사해서 쓰는데, 그 변수의 값이 변경된다면 , 복사본을 믿고 쓸수가 없게 된다. 
- 따라서 지역 변수에는 final이어야 하거나 final같이 동작해야한다는 제약 조건이 있는것이다.
- 하지만 인스턴스 변수는 힙에 존재하고 쓰레드끼지 공유도 가능하기에 별도로 복사할 필요도 없고 , 직접 힙에 접근해서 사용하면 된다. 



# 메소드, 생성자 레퍼런스
람다식이 하나의 메소드만 호출하는 경우, 메서드 참조를 통해 람다식을 간략히 할수 있음
클래스명::메서드명 또는 참조변수::메소드명

```java
//기존
Function<String, Integer> f = (String s) -> Integer.parseInt(s);

// 메서드 참조
function<String, Integer> f = Integer::parseInt;
```

생성자를 호출하는 람다식도 메소드 참조로 변환가능
```java
Supplier<MyClass> s = {} -> new MyClass(); //람다식
Supplier<MyClass> s = MyClass::new; 메서드 참조
```
배열로 생성할 경우
```java
Function<Integer, int[]> f = x -> new int[x]; // 람다식
Function<Integer, int[]> f2 = int[]::new; // 메서드 참조
```

# 참고
- https://coding-factory.tistory.com/265
- https://galid1.tistory.com/509
- http://www.tcpschool.com/java/java_lambda_concept
- https://ryan-han.com/post/java/java-lambda/
- https://perfectacle.github.io/2019/06/30/java-8-lambda-capturing/
