# 학습할 것 (필수)
- package 키워드
- import 키워드
- 클래스패스
- CLASSPATH 환경변수
- -classpath 옵션
- 접근지시자
---

# package 키워드
## 패키지
- 서로 관련이 있는 클래스나 인터페이스의 컴파일된 클래스(.class) 파일들을 한 곳에 묶어 놓은 것
- 패키지는 파일 시스템의 디렉터리(폴더)와 연관되엉 씨다.
- 하나의 패키지는 하나의 디렉터리에 저장된 클래스 파일들을 지칭
- 패키지는 jar 파일로 압축되어 존재 할 수 도 있다.

## 패키지명 관례
- 관례상 이름은 모두 소문자로 사용
- 웹사이트 주소를 반대로 패키지 이름을 부여한다. 
- 소스파일들 각 그룹 구분하기 위해 `.`으로 구분
- 최상위도메인/나라코드.회사명.프로젝트명
```java
    kr.co.naver.cafe
```

## 빌트-인 패키지(Built-in Package)
- 자바 개발자들이 사용 할 수 있도록 여러 가지 많은 패키지 및 클래스를 제공한다. 
- java.lang 패키지는 아주 기본적인 패키지여서 `import` 로 불러오지 않아도 java.lang의 클래스들을 불러온다.

## package 키워드 사용법
- 일반적으로 소스 파일에 처음오는 키워드 이다.
- 자바 소스파일에서 파일의 클래스나 클래스들이 속하는 패키지는 `package` 키워드로 지정
- `package` 선언
```java
    package org.hello.world;
```

# import 키워드
- 자바 소스파일 안의 패키지 클래스들을 사용하려면 `import` 선언을 하여 가져온다
```java
    //  org.hello.world 패키지로 부터 Hello 클래스만 가져온다.
    import  org.hello.world.Hello;

    // org.hello.world 패키지로 부터 모든 클래스를 가져온다.
    import  org.hello.world.*;
```

# 클래스패스
> 클래스패스(Classpth)는 사용자 정의 클래스 및  API 패키지의 경로를 지정하는 자바가상머신(JVM, Java Virtual Machine) 또는 Java 컴파일러의 매개변수
- JVM 이나 Java컴파일러에 사용자 정의 클래스의 클래스와 패키지 위치를 지정해주는 파라메터
- 자바가 클래스를 찾아 사용해야 하는데 클래스들이 어디 있는 지 위치를 지정해 주는 값
- 매개 변수는 명령 행 또는 환경 변수를 통해 설정 할 수 있다.

# -classpath(-cp) 옵션
- 컴파일러가 컴파일 하기 위해서 필요로 하는 참조할 클래스 파일들을 찾기 위해서 컴파일 시 파일 경로를 지정해주는 옵션
- JAVA 클래스 패스 설정할때의 2가지 방법
    - 1. 환경변수 `CLASSPATH`로 지정
    - 2. java or javac 명령행 옵션(-classpat)에서 경로를 지정

현재디렉토리(`.`) 와 `lib` 디렉토리에서 클래스 파일 찾으라고 옵션 지정    
- 구분자 
    - MacOS, Linux : `:`
    - Windows : `;`

```bat
    javac -classpath ".;lib" Hello.java
```

# 접근지시자

|접근지시자|클래스내부|동일패키지|상속받은클래스|이외의영역|
|---|---|---|---|---|
|private|o|x|x|x|
|default|o|o|x|x|
|protected|o|o|o|x|
|public|o|o|o|o|

- private
    - 클래스 내부에서만 접근 허용
- default(접근 지시자를 선언하지 않을 시)
    - 클래스 내부 + 동일 패키지내에서만 접근 허용
- proteced
    - 클래스 내부 + 동일 패키지의 상속받은 클래스에서만 접근 허용
- public 
    - 접근 제한 없음




# 출처
- 명품 Java programming
- https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%ED%8C%A8%ED%82%A4%EC%A7%80
- https://ko.wikipedia.org/wiki/%ED%81%B4%EB%9E%98%EC%8A%A4%ED%8C%A8%EC%8A%A4
- https://goateedev.tistory.com/169
- https://javakong.tistory.com/132