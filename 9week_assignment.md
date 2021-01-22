학습할 것 (필수)
- 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
- 자바가 제공하는 예외 계층 구조
- Exception과 Error의 차이는?
- RuntimeException과 RE가 아닌 것의 차이는?
- 커스텀한 예외 만드는 방법

## 예외처리(Exception Handling)
```
예외 처리(Exception Handling) 혹은 오류 처리(Trouble Shooting)란 실행 흐름상 오류가 발생했을 때 오류를 그대로 실행시키지 않고 오류에 대응하는 방법을 제시하는 개념이나 하드웨어 구조를 의미한다. 일반적으로 프로그래밍에서 프로그램이 실행 중 특정 문제가 발생했을 때 다른 처리 방식으로 흐름을 옮기는 개념으로 사용한다.
```



## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
### 예외 처리 3가지 블록
- Try 블록
    - 예외(Exception)이 발생할 가능성이 있는 코드가 들어가는 블록
- Catch 블록
    - 예외(Exception)를 처리코드가 들어가는 블록
    - Try 블록에서 예외(Exception) 발생 시 실행되는 블록
- Finally 블록
    - Try 블록에서 예외(Exception) 발생 여부와 상관 없이 무조건 실행되는 블록 
    - 생략 가능

### 예제 코드(try, catch, finally)
```java
    public static void main(String[] args) {
        try{// Try 블록
            System.out.println("result = " + doSomething(4,2));
            System.out.println("result = " + doSomething(4,0));

        }catch(Exception e) { // Catch 블록
            System.out.println("예외가 발생되었습니다.");
        }
        finally { // Fianlly 블록
            System.out.println("예외(Exception) 발생여부와 상관없이 무조건 실행");
        }
    }

    private static int doSomething(int num1, int num2) {
        int result  = 0;
        result = num1 / num2;
        return result;
    }
```

### 코드 실행 순서((try, catch,finally)
#### Exception 발생 시
- Try 블록 실행 -> Catch 블록 실행 -> Finally 블록 실행
#### Exception 미발생 시
- Try 블록 실행 -> Finally 블록 실행

### throw와 throws
- 둘다 Exception을 발생 시킨다.
- 차이점
    -  throws :  현재 메서드에서 자신을 호출 한 상위 메서드로 Exception을 발생 시킴 (현재 메서드를 호출한 메서드에게 예외처리에 대한 책임을 맡김)
    -  throw : 강제로 예외(Exception)을 발생시키고자 할떄 사용(현재 메서드 or 상위 메서드) - 프로그래머가 예외를 강제로 발생시켜 메서드 내에서 예외처리를 수행하는 것
```java
    public static void main(String[] args) {
        try {
            throwException();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    private static void throwException() throws Exception {
        System.out.println("예외를 발생할겁니다.");
        throw new Exception("강제로 예외 발생");
    }
```

## 자바가 제공하는 예외 계층 구조

![](./exception.png)
출처: https://howtodoinjava.com/best-practices/java-exception-handling-best-practices/

- Exception과 Error클래스가 Throwable 클래스를 상속 받고 있습니다.


## Exception과 Error의 차이는?

### Exception과 Error의 차이
 Exception :  프로그램 또는 개발자가 처리 할수 있는 예외
- Error : 시스템 레벨에서 비정상적인 상황이 생겼을 때  발생하기 때문에 개발자가 미리 예측 할 수 없고, 처리도 할 수 없습니다. 따라서 Error에 대한 처리를 신경 쓰지 않아도 됩니다.(예측이 불가능 )
    - 대표적으로 OOM(OutOfMemory)
    - Throwable의 하위 클래스
    - 시스템 레벨에서 발생

### Checked Exception , Unchekced Exception
- Checked Exception : 실행하기 전에 예측이 가능한 에러
    - IDE 가 컴파일할 때 체크 해줌
    - 필수 적으로 처리 해야 하는 예외(try~catch)
    - RuntimeException을 제외한 Exception의 하위 클래스
- Unchecked Exception : 실행하고 난 후에 알 수 있는 에러
    - 실행 시 객체 없는 경우(NullPointer), Array 사이즈가 맞는 않는 경우
    - RuntimeException 의 하위 클래스 

 ## RuntimeException과 RE가 아닌 것의 차이는?
 -  RuntimeException :  JVM의 정상적인 작동 중에 발생 할 수 있는 예외의 super class (실행중에 발생하는 예외)
    - 의도적으로 개발자가 발 생 시키는 경우도 있음(throw)
 - RuntimeException 아닌 것 : RuntimeException 부류를 제외한 Exception 클래스를 상속받은 나머지 클래스들
 - 큰 차이는 컴파일 시 예외 처리 체크를 하느냐 안하느냐의 차이


## 커스텀한 예외 만드는 방법
- 일반적으로 Exception 클래스를 상속받아 새로운 Exception 을 정의

예제
```java
123lass CustomRuntimeException extends RuntimeException{14124    CustomRuntimeException(String msg){
        super(msg);
    }
}

class CustimeException extends Exception{
	public BadBankingExcCustimeExceptioneption(String msg) {
		super(msg);
	}
}
```


출처 목록
- httpsQQWDQWDQWDD://jhkim2017.wordpress.com/2017/04/24/javaQDW-throws%EC%99%80-throw%EC%9D%98-%EC%B0%A8%EC%9D%B4/
- hDttps://namu.wiki/w/QWD%EC%98%88%EC%99%B8%20%EC%B2%9AS8%EB%A6%AC
- https://blog.kakaocdn.net/dn/cCfBzy/btqCF638xxV /42JuXKx2Or7Aiw5LPtMB0k/img.png
- https://howtodoinjava.com/best-practicesxczxcx/ ava-exception-handling-best-practices/24a