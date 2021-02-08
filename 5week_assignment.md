# 학습할 것 (필수)
- 클래스 정의하는 방법
- 객체 만드는 방법 (new 키워드 이해하기)
- 메소드 정의하는 방법
- 생성자 정의하는 방법
- this 키워드 이해하기
---


# 클래스 정의하는 방법

## 클래스(class)
- 객체를 정의하는 틀 또는 설계도
- 이러한 설계도를 가지고 , 여러 객체를 생성하여 사용
- 클래스는 필드(field) + 메소드(method)로 구성
    - 필드 : 객체의 상태를 나타냄
        - 클래스에 포함된 변수(variable)를 의미
    - 메소드 : 객체의 행동을 나타냄
        - 어떠한 특정 작업을 수행하기 위한 명령문의 집합

## 인스턴스(instance)
- 클래스를 사용하기 위해서는 우선 해당 클래스의 타입의 객체(object)를 선언해야함
- 클래스 인스턴스화 : 클래스로부터 객체를 선언하는 과정
- 인스턴스(instance) : 이렇게 선언된 해당 클래스 타입의 객체
- 인스턴스란 메모리에 할당된 객체를 의미
- 하나의 클래스로 부터 여러개의 인스턴스 생성할수 있음
- 생성된 인스턴스는 독립된 메모리 공간에 저장된 자신만의 필드를 가짐
- 하지만 해당 클래스의 모든 메소드는 해당 클래스에서 생성된 모든 인스턴스가 공유

## 클래스를 정의하는 방법
```java
접근제어자 class 클래스이름 {
    접근제어자 필드1의타입 필드1의이름;
    접근제어자 필드2의타입 필드2의이름;
    ...
    접근제어자 메소드1의 원형
    접근제어자 메소드2의 원형
    ...
};
```

# 객체 만드는 방법(new 키워드 이해하기)
new 연산자(new operator)
- 한 객체를 위한 메모리를 적재함으로써 클래스를 인스턴스화하며, 그 메모리에 대한 참조(reference)를 반환

new 연산자를 통해서 객체를 생성
Hello hello = new Hello();

## 메소드(method)

# 메소드를 정의하는 방법
접근제어자 반환타입 메소드이름(매개변수목록){ // 선언부
    // 구현부
}
- 1. 접근제어자 : 해당 메소드에 접근할 수 있는 범위 명시
- 2. 반환타입(return type) : 메소드가 모든 작업을 마치고 반환하는 데이터 타입을 명시
- 3. 메소드이름: 메소드를 호출하기 위한 이름을 명시
- 4. 매개변수목록(parameters): 메소드 호출 시에 전달되는 인수의 값을 저장하는 변수들을 명시 
- 5. 구현부: 메소드의 고유기능을 수행하는 명령문의 집합


# 생성자(constructor)
- 객체 생성과 동시에 인스턴스 변수를 원하는 값으로 초기화할 수 있는 메소드
- 해당 클래스의 이름과 같아야함

## 생성자의 특징
- 1. 생성자는 반환값이 없지만, 반환 타입을 void로 선언하지 않는다.
- 2. 생성자는 초기화를 위한 데이터를 인수로 전달 받을 수 있다.
- 3. 객체를 초기화하는 방법이 여러 개 존재할 경우에는 여러개의 생성자를 가질 수 있음
        - 즉, 생성자도 하나의 메소드이므로, 메소드 오버로딩이 가능하다는 의미

## 생성자 정의하는 방법
클래스이름(){...} // 매개변수가 없는 생성자 선언
클래스이름(인수1, 인수2,...){....} // 매개변수가 있는 생성자 선언

```java
Car(String modelName, int modelYear, String color, int maxSpeeds) {
    this.modelName = modelName;
    this.modelYear = modelYear;
    this.color = color;
    this.maxSpeed = maxSpeed;
    this.currentSpeed = 0;
}
```

## 기본생성자(default constructor)
- 모든 클래스는 하나 이상의 생성자가 정의되어 있어야 함
- 컴파일러는 컴파일 시 클래스에 생성자가 하나도 정의되어있지 않으면, 자동으로 기본생성자를 추가함
- 만약 매개변수를 가지는 생성자를 하나라도 정의했다면, 기본생성자를 자동으로 추가하지않음
```java
Car(){}
```


# this 키워드 이해하기
## this 참조변수
- 인스턴스가 바로 자기 자신을 참조하는데 사용하는 변수
- 해당 인스턴스의 주소를 가리키고 있음
- this 참조변수를 사용하여 인스턴스 변수에 접근 할 수 있음
- this 참조변수는 인스턴스 메소드에서만 사용가능. 클래스 메소드에서는 사용할 수 없음
- 모든 인스턴스 메소드에는 this 참조변수가 숨겨진 지역 변수로 존재
```java
class Car {
    private String modelName;
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;
    Car(String modelName, int modelYear, String color, int maxSpeed) {
        this.modelName = modelName;
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }
    ...
}
```
## this() 메소드
- 생성자 내부에서만 사용 가능
- 같은 클래스의 다른 생성자를 호출할떄 사용
- this() 메소드에 인수를 전달하면, 생성자 중에 메소드 시그니처가 일치하는 다른 생성자를 찾아 호출해줌 

메소드 시그니처
> 메소드시그니처(method signature) 란 ? 메소드의 이름과 메소드의 원형에 명시되는 매겨변수가 리스트를 가리킴

단, 한생성자에 다른 생성자를 호출할때에는 반드시 해당 생성자의 첫줄에서만 호출 할 수 있음

```java
class Car {
    private String modelName;
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;
    Car(String modelName, int modelYear, String color, int maxSpeed) {
        this.modelName = modelName;
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }
    Car() {
        this("소나타", 2012, "검정색", 160); // 다른 생성자를 호출함.
    }
    public String getModel() {
        return this.modelYear + "년식 " + this.modelName + " " + this.color;
    }
}
public class Method05 {
    public static void main(String[] args) {
        Car tcpCar = new Car(); System.out.println(tcpCar.getModel());
    }
}
``

# 출처
- http://www.tcpschool.com/java/java_class_intro
