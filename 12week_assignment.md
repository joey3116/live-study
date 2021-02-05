# 학습할 것 (필수)
- 애노테이션 정의하는 방법
- @retention
- @target
- @documented
- 애노테이션 프로세서
---

# 애노테이션 정의하는 방법

## 애노테이션이란?
> 메타데이터(metadata) 즉 컴파일 과정과 실행 과정에서 코드를 어떻게 컴파일하고 처리할 것인지를 알려주는 정보
- Annotation
- 주로 컴파일러에게 코드 문법 에러를 검사하도록 정보 제공, 런타임 시 특정 기능을 실행하도록 정보 제공
- 메타데이터: 데이터를 설명하는 데이터

## 애노테이션 정의하는 방법
- @interface 를 사용하여 정의
```java
public @interface HelloAnnotation{
    String msg();
    int number();
}
```
- 엘리멘트(Element)라는 멤버를 가질 수 있다.
- Element는 타입과 이름으로 구성되며 디폴트를 가질 수 있다.
- Element의 이름 뒤에 () 를 꼭 붙여야함


## 자바에서 기본적으로 제공되는 애노테이션
- @Deprecated
- @Override
- @SuppressWarnings
- @SafeVarargs
- FuncionalInterface

### @Deprecated
- 해당 클래스/메소드 등은 더이상 사용하지말라고 알려줄때 쓰임
- 컴파일러는 해당 애노테이션 보고 경고 메세지를 알려줌
```java
    @Deprecated
    public class Hello{
    }
```
### @Override
- 슈퍼클래스에 대해 오버라이드 되었다는걸 알려줌
- 슈퍼클래스와 매칭되지 않으면 에러 발생
- 강제는 아니지만 사용을 권고
```java
    public class Hello{
        public void print(){
            System.out.println("Hello")            
        }
    }

    public class HelloWorld extends Hello{
        @Override
        public void print(){
            System.out.println("Hello World")            
        }
    }

```

### @SuppressWarnings
- 컴파일러가 정적분석을 진행 할떄 오류가 아니라고 마킹해주는 역할
```java
    @SuprressWarnings("rawtypes") // 하나만 적용 할 경우
    @SuprressWarnings({"rawtypes"}) // 하나만 적용 할 경우
    @SuprressWarnings({"rawtypes","unchecked"}) // 두개 이상 적용 할 경우
    public void hello(){
        ...
    }
```

|옵션|설명|
|---|---|
|all|모든경고|
|cast|캐스트 연산자 관련 경고|
|dep-ann|사용하지 말아야 할 주석 관련 경고|
|deprecation|사용하지 말아야 할 메서도 관련경고|
|fallthrough|swith문에서 break 누락 관련 경고|
|finally|반환하지 않는 finally 블럭 관련 경고|
|null|null 분석 관련 경고|
|rawtypes|제너릭을 사용하는 클래스 매개변수가 불특정일때의 경고|
|unchecked|검증되지 않은 연산자 관련 경고|
|unused|사용하지 않는 코드 관련경고|

## @SafeVarargs
- 제너릭 같은 가변인자 매개변수를 사용할 떄 경고를 무시(자바7이상)

## @FuncionalInterface
- 람다 함수등을 위한 인터페이스를 지정
- 메소드가 없거나 두개 이상이되면 컴파일 오류가 남(자바8이상)


# Meta Annotations
- 기본 애노테이션 외 메타 애노테이션(Meta Annotation)이 있다.
- 이 애노테이션들을 사용하여 커스텀 애노테이션을 만들수 있음
- @Retention : 애노테이션의 범위 지정- 어떤 시점까지 영향을 미치는지 결정 
- @Documented : 문서에도 애노테이션 정보가 표현되도록 지정
- @Target : 애노테이션이 적용할 위치를 지정
- @Inherited : 자식클래스가 애노테이션을 상속 받을 수 있다.
- @Repeatable : 반복적으로 애노테이션을 선언할 수 있게 함

# @Retention
- 언제까지 유지할지 알려주는 애노테이션
- 어느 시점까지 유효한지를 명시해줘야함
```java
    @Retention(RetentionPolicy.RUNTIME)
```
## RetnetionPolicy
### Retention에 3가지 속성
- SOURCE
- CLASS
- RUNTIME
### SOURCE
- 커스텀 애노테이션을 주석처럼 사용하고 싶을 때 주는 옵션
- 컴파일된 .class에 해당 애노테이션 정보가 사라짐
- 즉 소스까지만 유지

### CLASS
- @Retention 의 기본값
- 컴파일된 .class 파일에서도 유지됨
- 즉 런타임 시 클래스를 메모리로 읽어오면 해당정보는 사라짐

### Runtime
- 클래스를 메모리에 읽어왔을 때까지 유지
- 코드에 이 정보 바탕을 특정 로직을 실행 할 수 있다.


# @Target
- 애노테이션이 적용할 위치를 선택
- 애노테이션이 생성될 수 있는 위치를 지정
- 기본 Element value는 ElementType 배열 값으로 가진다.
- 애노테이션이 적용될 대상을 복수개로 지정하기 위함

|ElementType 열거 상수| 적용대상|
|----------------------|----------------------|
|TYPE|클래스, 인터페이스, 열거타입|
|ANNOTATION_TYPE|애노테이션|
|FIELD|필드|
|CONSTRUCTOR|생성자|
|METHOD|메소드|
|LOCAL_VARIABLE|지역변수|
|PACKAGE|패키지|

```java
@Target({ElementType.TYPE, ElementType.FIELD})
public @interface HelloAnn(){
}
```

# @Documented
- 해당 애노테이션을 javadoc에 포함


# 애노테이션 프로세스
- 컴파일 타임에 애노테이션을 분석
- 붙여진 애노테이션에 따라서 클래스를 만들어 냄

> 1. 애노테이션 클래스를 생성
> 2. 애노테이션 파서 클래스를 생성
> 3. 애노테이션을 사용
> 4. 컴파일하면, 애노테이션 파서가 애노테이션을 처리
> 5. 자동 생성된 클래스가 빌드 폴더에 추가됨
- 애노테이션이 붙은 클래스를 정보를 트리 구조로 참조 할 수 있다.(AST)
    - abstract syntax tree
- Annotation Parser classese는 오직 프로젝트 컴파일 할때만 필요(빌드 끝나면 쓰이지 않음)
