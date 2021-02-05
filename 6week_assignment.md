# 학습할 것 (필수)
- 자바 상속의 특징
- super 키워드
- 메소드 오버라이딩
- 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
- 추상 클래스
- final 키워드
- Object 클래스
---

# 자바 상속의 특징
> 상속(inheritance)
> 기존의 클래스에 기능을 추가하거나 재정의하여 새로운 클래스를 정의하는 것을 의미

- 상속은 캡슐화, 추상화와 더불어 객체 지향 프로그래밍을 구어하는 중욕한 특징 중 하나
- 기존에 정의되어있던 클래스 : 부모 클래스(parent class) 또는 상위클래스(super class), 기초 클래스(basic class) 라고도 함
- 상속에 통해 새롭게 작성된 클래스 : 자식클래스(child class) 또는 하위클래스(sub class), 파생클래스(derived class) 라고 함

## 상속의 장점
- 기존의 작성된 클래스를 재활용할 수 있다.
- 자식 클래스 설계 시 중복되는 멤버를 미리 부모 클래스에 작성해 놓으면, 자식 클래스에서는 해당 멤버를 작성하지 않아도 된다.
- 클래스 간의 계층적 관계를 구성함으로써 다형성 문법적 토대를 마련함

## 자바의 상속의 특징
- 다중 상속을 지원하지 않는다.
- 상속의 횟수를 제한을 두지 않는다.
- 계층 구조의 취상위에 있는 클래스는 `java.lang.Object` 클래스다.

# `super` 키워드
- 부모크랠스로부터 상속받은 필드나 메소드를 자식 클래스에서 참조하는데 사용하는 참조변수
- 부모 클래스의 멤버와 자식 클래스의 멤버의 이름이 같은 경우 `super` 키워드를 사용하여 구별
- this와 마찬가지로 대상은 인스턴스 메소드뿐이며, 클래스 메소드는 사용 할 수 없다.

## super()메소드
- 부모클래스의 생성자를 호출할 때 사용
- 부모 클래스의 멤버를 초기화하기 위해서 자식클래스의 생성자에서 부모클래스의 생성자까지 호출해야함
- 이러한 부모클래스의 생성자 호출은 모든 클래스의 부모 클래스인 Object 클래의 생성자까지 계속 거슬러 올라가며 수행
- 컴파일러는 부모클래스의 생성자를 명시적으로 호출하지 않는 모든 자식 클래스의 생성자의 첫줄에 자동으로 다음과 같이 추가
    - 클래스에 생성자가 하나도 정의 되지 않는 경우만 자동으로 추가해줌
```java
super();
```
-아래와 같이 Child 생성자에서 super() 호출하면 에러 발생
    - 부모 클래스에서 기본생성자가 추가되지 않았게 떄문 
    - 따라서 매개변수를 가진 생성자를 선언해야 하는 경우 , 기본 생성자를 명시해주는게 좋음
```java
class Parent{
    int a;
    /**
    Parent(){ a=10;}
    **/
    Parent(int n ){ a = n;}
}

class Child extends Parent{
    int b;
    Child(){
        super(); //에러
        b =20;
    }
}
```


# 메소드 오버라이딩
> method overriding
> 상속관계에 있는 부모 클래스에서 이미 정의된 메소드를 자식 클래스에서 같은 시그니쳐를 갖는 메소드로 다시 정의하는 것

- 부모로 부터 상속받은 메소드를 재정의 하여 사용하는것을 의미
- 부모의 private 멤버를 제외한 모든 메소드를 상속 받는다.

## 오버라이딩의 조건
- 메소드의 선언부는 기존의 메소드와 완전히 같아야함
    - 메소듸 반환타입은 부모의 클래스의 반환타입으로 타입 변환할 수 있는 타입이라면 변경 가능
- 부모클래스의 메소드보다 접근 제어자를 더 좁은 범위로 변경 할 수 없음
- 부모클래스의 메소드보다 더 큰 범위의 예외를 선언 할 수 없음

## 오버로딩과 오버라이딩
- 오버로딩(overloading)은 새로은 메소드를 정의하는 것
- 오버라이딩(overrriding)은 상속 받은 기존 메소드를 재정의 하는 것

# 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)
Method Dispatch
- 어떤 메소드를 호출할 지 결정하여 실제로 실행시키는 과정

## Static Dispatch
- 컴파일 시점에 컴파일러가 특정 메소드를 호출할 것이라는 걸 명확하게 알고 있는 경우
- 바이트코드에도 이 정보가 그대로 남아 있음
- 런타임(실행시점)이 되지 않아도 미리 결정하는 개념
- 오버로딩경우 , 인자의 타입, 리턴타입 등 어떤 메소드가 호출될지 명확하기 때문에
- 부모 클래스가 존재 하더라도, 자식클래스로 선언하고 자식클래스로 인스턴스를 만들었다면 static Dispath에 해당
    - Child child = new Child();

## Dynamic Dispatch
- dynamic = runtime
- dispatch = 어떤 메소드를 호출 할지 결정하는 것
- 상위 개념인 interface 또는 Abstract class 에 정의된 abstract method를 호출하는 경우에 해당
- Parnet parent = new Child();

```java
    static class Super {
        void print() {
            System.out.println("super's print");
        }
    }

    static class Sub1 extends  Super{
        @Override
        void print() {
            System.out.println("sub1's print");
        }
    }

    static class Sub2 extends Super{
        @Override
        void print() {
            System.out.println("sub2's print");
        }
    }

    public static void main(String[] args) {
        Super reference = new Super(); // 1)
        reference.print();
        reference = new Sub1(); // 2)
        reference.print();
        reference = new Sub2(); // 3)
        reference.print();
    }
```
> super's print  
> sub1's print  
> sub2's print  

## Double Dispathc
- 런타임 디스패치를 두번 시도하는 것
- 예) 
텍스트와 사진 두가지의 포스트를 올릴 수 있고, 내가 하는 모든 SNS에 포스트를 한꺼번에 올리는 코드를 작성한다고 가정
```java
 interface Post { void postOn(SNS sns); }

    static class Text implements Post {
        @Override
        public void postOn(SNS sns) {

            if(sns instanceof Instagram) {
                // logic which is applicable to Instagram
            }
            else if (sns instanceof Twitter) {
                // logic which is applicable to Twitter
            }

        }
    }

    static class Picture implements Post {
        @Override
        public void postOn(SNS sns) {
            if(sns instanceof Instagram) {
                // logic which is applicable to Instagram
            }
            else if (sns instanceof Twitter) {
                // logic which is applicable to Twitter
            }
        }
    }

    interface SNS { }
    static class Instagram implements SNS{ }
    static class Twitter implements SNS{ }
```
위 코드는 문제점은 다른 타입의 SNS가 추가될때 마다 SNS의 인스턴스 타입 판별하는 if 문을 계속 추가해줘야 한다.
만약 또다른 SNS 인 Facebook 로직을 Text와 Post에 구현하고 if문에 추가하지 않았다면 Facebokk 업로드 로직을 처리할 수가 없는 상태.
즉, 코드 관리의 cost가 있는 코드이다.
이 문제를 double dispatch를 통해 해결

```java
 interface Post { void postOn(SNS sns); }

    static class Text implements Post {
        @Override
        public void postOn(SNS sns) {
            sns.post(this);
        }
    }

    static class Picture implements Post {
        @Override
        public void postOn(SNS sns) {
            sns.post(this);
        }
    }

    interface SNS {
        void post(Text post);
        void post(Picture post);
    }
    static class Instagram implements SNS{
        @Override
        public void post(Text post) {
           // text upload logic which is applicable to Instagram
        }

        @Override
        public void post(Picture post) {
        	//picture upload logic which is applicable to Instagram
        }
    }
    static class Twitter implements SNS{
        @Override
        public void post(Text post) {
          	// text upload logic which is applicable to Twitter
        }

        @Override
        public void post(Picture post) {
            // picture upload logic which is applicable to Twitter
        }
    }
```
결과적으로 새로운 Facebook sns를 구현해 업로드하는 클라이언트 코드를 작성해도 Post 코드를 건드릴 필요가 없다. Facebook 코드만 충실히 구현되있으면 된다.
앞코드보다 cost가 확 줄고, 확장에 대해선 열리고 수정에 대해 닫힌 객체지향성을 지킨코드가 되었다.



# 추상클래스
> 추상(abstract) : 실체 간에 공통되는 특성을 추출한 것  
> 추상클래스 : 실체 클래스들의 공통적인 특성을 추출해 선언한 클래스
```java
public abstract class 클래스{
    ...
}
```
- 실체 클래스 : 객체를 직접 생성 할 수 있는 클래스
- 필드, 생성자, 메소드 선언할 수 있음
- 객체를 직접 생성해서 사용 할 수 없음(new 연산자를 사용해 인스턴스 생성 하지 못함)
```java
Hello hello = new Hello(); //오류
```
- new 연산자 통해 직접생성자를 호출 할 수 없지만, 자식 객체가 생성될때 super()를 호출해서 추상 클래스 객체를 생성하므로 추상클래스도 생성자가 반드시 있어야 함
```java
public abstract class Hello{
    public String msg;

    public Hello(String msg){
        this.msg = msg;
    }

    public void print(){
        System.out.println(this.msg);
    }
}
```

## 추상클래스의 용도
- 실체 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적
- 실체 클래스를 작성 할 때 시간을 절약
    - 공통적인 필드와 메소드는 추상클래스에서 모두 선언해두고, 실체 클래스마다 다른 점만 실체 클래스에 선언

## 추상 메소드와 오버라이딩
- 상속받은 모든 실체 클래스들이 메소드의 내용이 동일하다면 추상 클래스에서 메소드를 작성
- 하지만 선언만 통일하고, 내용은 실체 클래스마다 달라야 하는 경우 추상메소드를 선언
```java
[public | protected] abstract 리턴타입 메소드명(매개변수,...);
```

```java
public abstract class Hello{
    public abstract void print();
}

public class HiHello extends Hello{
    @Override
    public void print(){
        System.out.println("HI");
    }
}

public class KOHello extends Hello{
    @Override
    public void print(){
        System.out.println("안녕하세요");
    }
}

정의한 메소드를 다양한 방식으로 호출 할 수 있음
```java
public calss Main{
    public static void main(String[] args){
        KOHello kohello = new KOHello();
        HiHello hihello = new HiHello();
        kohello.print();
        hihello.print();

        Hello hello = null;
        hello = new KOHello();
        hello.print();
        hello = new HiHello();
        hello.print();

        printMSG(new KOHello());
        printMSG(new HiHello());
    }

    public static void printMSG(Hello hello){
        hello.print();
    }

}

```



# `final` 키워드
- 최종상태이고, 수정될수 없음을 선언
- 클래스, 필드, 메소드에 선언
- 클래스에 선언시, 자식 클래스를 만들수 없음
- 메소드에 선언시, 오버라이딩 할 수 없음
- 필드에 선언시, 초기값 설정 후, 값을 변경 할 수 없음


# Object 클래스
- 자바에서 Object 클래스는 모든 클래스의 부모 클래스가 되는 클래스
- 모든 클래스는 자동으로 Object 클래스의 모든 필드와 메소드를 상속받게됨
- 모든 클래스는 `extends` 키워드를 사용하여 Object 클래스 상속을 명시 하지 않아도 Object의 모든 멤버를 자유롭게 사용할 수 있음

## Object 클래스의 메소드
|메소드|설명|
|----|----|
|protected Object clone()|해당 객체의 복제본을 생성하여 반환|
|boolean equals(Object obj)|해당 객체와 전달받은 객체가 같은지 여부를 반환함.|
|protected void finalize()|해당 객체를 더는 아무도 참조하지 않아 가비지 컬렉터가 객체의 리소스를 정리하기 위해 호출함|
|Class<T> getClass()|해당 객체의 클래스 타입을 반환함|
|int hashCode()|해당 객체의 해시 코드값을 반환함|
|void notify()|해당 객체의 대기(wait)하고 있는 하나의 스레드를 다시 실행할 때 호출함|
|void notifyAll()|해당 객체의 대기(wait)하고 있는 모든 스레드를 다시 실행할때 호출함.|
|String toString()|해당 객체의 정보를 문자열로 반환함|
|void wait()|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행할떄 까지 현재 스레드를 일시적으로 대기(wait) 시킬 때 호출함|
|void wait(long timeout)|해당 객체의 다른 스레드가 notify()나 notifyAll()메소드를 실행하거나 전달받은 시간이 지날때까지 현재 스레드를 일시적으로 대기(wait) 시킬때 호출함|
|void wait(long timeout, int nanos)|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지나거나 다른 스레드가 현재 스레드를 인터럽트(interrupt) 할 때까지 현재 스레드를 일시적으로 대기(wait) 시킬때 호출함|





# 참고
- http://www.tcpschool.com/java/java_inheritance_concept
- https://scshim.tistory.com/210
- https://defacto-standard.tistory.com/413
- https://velog.io/@maigumi/Dynamic-Method-Dispatch
- https://www.youtube.com/watch?v=s-tXAHub6vg&feature=youtu.be
- 이것이 자바다
