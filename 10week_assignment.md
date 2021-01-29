# 학습할 것 (필수)
- Thread 클래스와 Runnable 인터페이스
- 쓰레드의 상태
- 쓰레드의 우선순위
- Main 쓰레드
- 동기화
- 데드락


# Thread 클래스와 Runnable 인터페이스
## Thread 란?
- `실`을 뜻하는 영어 단어
- `쓰레드`라고 적는다
- 프로세스보다 작은 실행 흐름의 최소단위
   - <details>
        <summary>
        자세히
        </summary>
        - 예전에는 프로그램 실행하는 흐름이 프로세스 뿐이었으나, 하나의 프로그램에서 복잡한 동시 작업을 요구하기 시작하면서 하나의 프로그램이 여러개의 프로세스를 만들어야 했는데, 프로세스 특성상 하나의 프로그램이 이러한 동시 작업을 수월하게 할 수 없어 <strong>프로세스보다 더 작은 실행 단위</strong> 가 만들어 지게되었다.
    </details>
- 하나의 프로세스에서 여러개의 쓰레드가 메모리 공유하여 작동 할수 있다.
    - <details>
        <summary>
        자세히
        </summary>
        <ul>
            <li> 생성과 속도가 빠르다</li>
            <li> 적은 메모리 점유</li>
            <li> 정보 교환이 쉬움</li>
            <li> Context Switching 부하가 적지만 자원 선점과 동기화 문제</li>
        </ul>
    </details>
- 둘 이상의 쓰레드를 가진 프로세스 `멀티쓰레드 프로세스(multi-threaded process)`

## 쓰레드와 프로세스의 차이점
- 프로세스 
    - 자바에는 프로세스 개념이 존재하지 않는다.
    - 서로 완벽히 독립적인 공간을 가진다.
    - 각자가 각자의 스텍과 데이터 영역을 가지고, 보호받는다.
    - 시작 시, OS에서 PCB와 메모리 공간 할당받고 초기화 과정 필요하다.
    - 다른 프로세스의 영역을 들여다 볼수 없어 번거로운 과정 거쳐야한다.(프로세스간 통신, 공유메모리 생성)
    - 한 프로세스가 비정상적으로 종료해도 다른 프로세스에 영향이 거의 없다.
- 쓰레드
    - 스텍은 따로따로이지만, 코드영역과 데이터 영역은 하나로 공유
    - 데이터 영역에 속하는 변수를 통해서 쉽고 빠르고 편하게 통신
    - 쓰레드 하나가 잘못된 연산이나 버그등으로 비정상 종료시, 같은 프로세스 속한 다른 쓰레드 모두 강제 종료

## Thread 클래스와 Runnable 인터페이스
### 쓰레드를 생성 하는 2가지
 - Thread 클래스 상속
 - Runnable 인터페이스 구현

Runnable 사용 추천
- Thread 클래스 상속 받으면, 다른클래스를 상속 받지 못하기 때문

- 2가지 방법 모두 `run()` 메소드를 오바라이딩 하여 작성  
- `Runnable 인터페이스 구현` 시,  Thread 객체의 생성자로 넘겨주어야 한다.

쓰레드 생성 예제
```java
public class HelloThread{

    public static void main(String[] args) {
        //쓰레드 생성
        ThreadEtds threadEtds = new ThreadEtds();
        Thread runnableImpl = new Thread(new RunnableImpl()); //`Runnable 인터페이스 구현` 시,  Thread 객체의 생성자로 넘겨주어야 한다.

        // 쓰레드 시작
        threadEtds.start();
        runnableImpl.start();
    }
}

// Thread 클래스 상속 
class ThreadEtds extends  Thread{
    int n = 0 ;
    @Override
    public void run() {
        for(int i =0 ; i<10; i++){
            System.out.println("ThreadEtds : "+ i);
        }
    }
}
// Runnable 인터페이스 구현
class RunnableImpl implements Runnable{

    @Override
    public void run() {
        for(int i =0 ; i<10; i++){
            System.out.println("RunnableImpl : "+ i);
        }
    }
}

```

# 쓰레드의 상태
- NEW
- RUNNABLE
- WAITING
- TIMED_WAITING
- BLOCK
- TERMINATED

|상태|설명|
|---|---|
|NEW|쓰레드가 생성 되었지만, 아직 실행할 준비가 되지 않은 상태,start()메소드가 호출되면 RUNNALBE 상태가 된다.|
|RUNNABLE|쓰레드가 실행되고 있거나, 실행 준비가 되어 스케줄링을 기다리는 상태|
|WAITING|wait()메소드를 호출하고 무한 대기 하면서, 다른 쓰레드가 notify(), noifyAll() 을 불러주기를 기다리는 상태. 쓰레드 동기화 위해 사용|
|TIMED_WAITING|쓰레다가 sleep(int n)을 호출하여 n밀리초 동안 잠을 자고 있는 상태|
|BLOCK|사용하고자 하는 객체의 락이 풀리때 까지 기다리는 상태(동기화 블럭에 의해 일시정지된 상태)|
|TERMINATED|쓰레드가 종료한 상태|


# 쓰레드의 우선순위
- JVM은 철저히 우선순위를 기반으로 쓰레드를 스케줄링한다.
- 가장 우선순위의 쓰레드를 먼저 실행
- 우선순위 범위 : 1 ~ 10(default: 5)
- 쓰레드를 생성한 쓰레드로 부터 우선순위를 상속 받는다.( main 메소드는 5)
- 우선순위가 높고 낮은 절대적인것이 아니라 상대적이다.
- 쓰레드 스케줄링은 우선순위 (Priority) 방식과 순환할당(Round-Robin) 방식 사용(동일한 우선순위인 경우  순환할당방식으로 돌아가면서 실행)
    - 우선순위 방식 : 우선순위가 높은 쓰레드가 실행 상태를 더 많이 가지도록 하는 것(setPriority()를 통해 설정)
    - 순환할당 방식 : 시간 할당량(Time Slice)을 정해서 하나의 쓰레드를 정해진 시간만큼 실행하고 다시 다른 쓰레드를 실행하는 방식
    - 순환할당 방식은 JVM에 의해 결정 되기때문에 임의로 수정 할 수 없다.


Thread 클래스의 우선순위 
```java
    // 우선순위 지정
    public final void setPriority(int newPriority) {
        ...
    }
    // 우선순위 값 얻기
    public final int getPriority() {
        return priority;
    }
    // 우선순위 최솟값
    public static final int MIN_PRIORITY = 1;
    // 우선 순위 보통값
    public static final int NORM_PRIORITY = 5;

    // 우선 순위 최댓값
    public static final int MAX_PRIORITY = 10;
```

# Main 쓰레드
## 자바 쓰레드 종류
- 데몬 쓰레드(daemon thread)
    - JVM이 스스로 필요에 의해 사용하는 쓰레드(가비지 컬렉션 쓰레드)
    - 응용프로그램에서 작성한 쓰레드를 데몬 쓰레드로 표시하여 JVM이 데몬 쓰레드로 인식하게 할 수 있다.
- 일반 쓰레드(non-daemon thread)
    - 응용프로그램에서 작성한 쓰레드(대표적으로 main 쓰레드)
- 응용프로그램의 일반 쓰레드가 모두 종료하면 , 데몬 쓰레드가 살아 있더라도 JVM은 스스로 종료
## Main 쓰레드
- main() 메소드는 JVM에 의해 생성된 Main 쓰레드에 의해 호출됨
- Main 쓰레드는 JVM이 데몬 쓰레드가 아닌 일반 쓰레드로 등록한다. 
- 자바 응용프로그램의 main() 메소드가 실행되는 순간 2개의 쓰레드가 존재하게 된다.(가비지 컬렉션 쓰레드, Main 쓰레드)
- 프로세서의 종료
    - 싱글쓰레드 : Main 쓰레드가 종료하면 프로세스도 종료
    - 멀티쓰레드 : 실행 중인 쓰레드가 하나라도 있다면, 프로세스는 종료되지 않는다.


# 동기화
## 쓰레드 동기화의 필요성
멀티쓰레드는 다수의 작업을 동시에 하게 하는데, 다수의 쓰레드가 공유 자원 혹은 공유데이터에 동시에 접근하여 문제 발생

## 쓰레드 동기화
- 공유데이터에 접근하고자 하는 다수의 쓰레드가 서로 순서대로 충돌 없이 공유 데이터를 배타적으로 접근하기 위해 상호 협력(coordination)하는 것
- 공유데이터에 대한 접근은 배타적이고 독점적으로 이루어져야 한다.

## 임계영역(Critical Section)
- 공유데이터에 대해 하나의 쓰레드만 접근 가능한 영역
- 공유데이터를 다루는 프로그램 코드

## 쓰레드 동기화를 위한 `synchronized`
- 1. 하나의 메소드 전체에 임계영역으로 지정 하는 방법
- 2. 임의의 코드 블록을 임계영역으로 지정 하는 방법
- synchronized 블록은 진입할 때 락을 걸고 빠져 나올때 락을 푸는 동작이 자동으로 이루어짐
- 먼저 sychnoized 블록에 진힝ㅂ하는 쓰레드가 락을 걸고 소유하며, 락을 소유하지 못한 다른 쓰레드는 synchronized 블록 앞에서 락을 소유할때 까지 대기

## Lock
공유 객체에 여러 쓰레드가 동시에 접근하지 못하도록 위한것

`synchronized` 예제
```java
    //하나의 메소드 전체에 임계영역으로 지정 
    synchronized void add(){
        int n = getCurrentSum();
        n+1=10;
        setCurrentSum();
    }
    //임의의 코드 블록을 임계영역으로 지정
    void add(){
        ...
        synchronized(this){
            int n = getCurrentSum();
            n+1=10;
            setCurrentSum();
        }
        ...
    }
```

### wait(), notify(), notifyAll()를 이용한 쓰레드 동기화
java.lang.Object 클래스는 쓰레드 사이에 동기화를 위한 3개의 메소드 제공
- wait()
- notify()
- notifyAll()

- wait()
    -  다른 쓰레드가 이 객체의 notify()를 불러줄떄 까지 대기
- notify()
    - 이 객체에 대기 중인 쓰레드를 꺠워 RUNNABLE 상태로 만듬
    - 2개 이상의 쓰레드가 대기중이라도 오직 한개의 쓰레드만 꺠워 RUNNABLE 상태로 함
- notifyAll()
    - 이 객체에 대기 중인 모든 쓰레드를 꺠우고 모두 RUNNABLE 상태로 함

# 데드락(Deadlock)
- 두개 이상의 쓰레드에서 작업을 완료하기 위해 상대의 작업이 끝나야 하는 상황(다수의 Thread가 서로의 Lock을 기다리는 상황)
- Java Application에서 데드락이 발생 했을 떄, 정상으로 되돌리면 Application 을 재시작해야 한다.

## 데드락 생기는 과정 예제
```java
// 데드락 위험이 있는 코드
// - 쓰레드 A가 락 left을 확보한 상태에서 락 right을 확보하려 대기
// - 쓰레드 B가 락 right을 확보한 상태에서 락 left을 확보하려고 대기
// - 양쪽 쓰레드 A, B는 서로 락을 풀기를 영훤히 기다리게 됨
public class LeftRightDeadlock {
  private final Object left = new Object();
  private final Object right = new Object();
  public void leftRight() {
    synchronized (left) {
      synchronized (right) {
        doSomething();
      }
    }
  }
  public void rightLeft() {
    synchronized (right) {
      synchronized (left) {
        doSomethingElse();
      }
    }
  }
}
```

## 데드락 예방하기
- 1. Lock이 발생하는 순서를 정해 놓는다.
- 2. 오픈 호출
- 3. 락의 시간 제한

### 1. Lock이 발생하는 순서를 정해 놓는다.
프로그램 내부의 모든 쓰레드에서 필요한 락을 모두 같은 순소로만 사용한다면, 락 순서에 의해 데드락이 발생하지 않는다.{자바 병렬 프로그래밍 P.307}
```java
// 해결 전: 데드락 위험이 있는 코드
//  fromAccount와 toAccount의 순서만 달리해서 동시 호출이 일어나면 데드락이 걸릴 확률이 증가
public void transferMoney (Account fromAccount, Account toAccount, DollarAmount amount) {
  synchronized (fromAccount) {
    synchronized (toAccount) {
      if (fromAccount.getBalance().compareTo(amount) < 0) {
        throw new InsufficientFundsException();
      } else {
        fromAccount.debit(amount);
        toAccount.credit(amount);
      }
    }
  }
}
```

```java
// 해결 후: Lock이 발생하는 순서를 제어한 경우
private static final Object tieLock = new Object();

public void transferMoney(final Account fromAccount, final Account toAccount, final DollarAmount amount) {

  class Helper {
    public void transfer() {
      if (fromAccount.getBalance().compareTo(acmount) < 0) {
        throw new InsufficientFuncsException();
      } else {
        fromAccount.debit(amount);
        toAccount.credit(ammount);
      }
    }

  }

  int fromHash = System.identityHashCode(fromAccount);
  int toHash = System.identityHashCode(toAccount);

  if (fromHash < toHash) {
    synchronized(fromAccount) {
      synchronized(toAccount) {
        new Helper().transfer();
      }
    }
  } else if (fromHash > toHash) {
    synchronized(toAccount) {
      synchronized(fromAccount) {
        new Helper().transfer();
      }
    }
  } else {
    synchronized (tieLock) {
      synchronized(fromAccount) {
        synchronized(toAccount) {
          new Helper().transfer();
        }
      }
    }
  }
}
```
### 2. 오픈 호출
락을 전혀 확보하지 않은 상태에서 메소드를 호출하는 것(쓰레드 안정선을 확보하기 위해 캡슐화 기법 encapsulation 사용하는 것과 비슷)
 - 특정 락을 확보한 상태에서 다른 메소드를 호출한다는 것은 파급효과가 날 수 있음(호출한 메소드 내부에서 어떤일이 일어나는지 알지 못하기 때문에)

### 3. 락의 시간 제한
락 시간을 제한할 수 있는 `Lock클래스`의 `tryLock 메소드`를 사용(`synchronized`는 암묵적인 락)
- 암묵적인 락은 락은 락을 확보할떄까지 영원히 기달린다.
- Lock클래스의 명시적락은 일정 시간을 정해 두고 그시간 락을 확보하지 못하면 tryLock 메소드가 오류를 발생시키도록 할 수 있다.

# 출처
- 명품 Java Programming
- https://namu.wiki/w/%EC%8A%A4%EB%A0%88%EB%93%9C
- https://coding-factory.tistory.com/569
- https://icednut.space/2016/08/06/20160806-about_deadlock



자바 병렬프록 프로그래밍 - 에이콘 강철구 옮김
톰캣 최종분석 -  지루함 별로 추천은 아님  읽다 말음

엘레강트 - 비추

롬복 애노테이션
- @SneakyThrows 
- interrupt();  쓰레드 중단이 아니라. 꺠우는거?
- stop(); //deprecated됨
- 쓰레드가 끝났다 아니다는 우리(개발자)가 상태 보고 직접하는(쓰레드 종료) 거지 JVM이 하지 않음 그래서 STOP()이 없음 
- 쓰레드를 쓸일은 거의 없음 - 톰캣, 언더토우, 제티 등 컨테이너에 있기 때문에 
- 서버 리소스를 극한으로 활용할떄 쓰레드를 사용 - 하지만 컨테이너가 대신하기 떄문에 

- coonection pool( 요새는 CPU갯수만큼만들고) 에 들어 있는 쓰레드가 처리 
coonection pool( 요새는 CPU corea 갯수만큼만들고) 하고 nio connector 으로 처리 
요즘 컨테이너들응 nio connector 를 씀

ExecutorService service = Excutros.newFiexTHreadOol(8);

CountDownLatch = new CountDownLAtch(15);
  for(){
    service.executre(new Runnable(){
      @Override
      public void (){
        // .....
        latch.coutDown();
      }
    })
  }
  latch.awit();
  service.shutdown()



동시성과 병렬성 
- 동시성 : 왔다 갔다 하는거(밥먹다가 책보다가 자다가 짧은 시간안에 )
- 병렬성 : 같이 하는거 (밥먹으면서 유투브 보는거)


concurrent 다른 모델
 ** Actor model- java에서는  아카(Akka)
또 다른 형태의 concurrent  모델 

stm(Software transactional memory) - java에서는 클로저(Clojure)


자바 챔피언- 오라클에서 인정하는 커뮤니티에 발전하고 그런걸로 
양수열, 이희승- 네티의 만든사람 - Triston lee

병렬프로그래밍의 단어
- critical path
  - 동시에 실행하는거 중에 가장 오래 걸리는 거 
  - 전체 실행시간에 영향을 미치는 애들 
  - 전체 수행시간을 줄이기 위해 가장 우선적으로 개선할 부분.

- 경쟁상태(race confition)
  - 공유하는 자원이 있고, 어떤게 먼저 접근하는거에 따라 결과가 달라지는 경우


데드락 보는 법
viusal vm > 쓰레드 덤프를 통해 확인 가능 

heap dump-메모리덤프(뜰려면 나도 그만큼 램이 있어야 한다.), trhead dump

yadon 님꺼 확인해보기

무어에 법칙 : 24개월마다 2배 클록이 높아진다.?
free launch over : 더이상 클록이 빨라지지 않는다.