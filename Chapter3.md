Chapter3 다형성과 추상 타입
==================================================================================

## 다형성과 상속
- 다형성은 한 객체가 여러 가지 모습을 갖는다는 것을 의미한다. 여기서 모습이란 타입을 뜻하는데, 즉, 다형성이란 한 객체가 여러 타입을 가질 수 있다는 것을 뜻한다.

```java
public class Plane {
    public void fly() {
        // 비행
    }
}

public interface Turbo {
    publid void boost();
}

public class TurboPlane extends Plane implements Turbo {
    public void boost() {
        // 가속
    }
}
```

```java
TurboPlane tp = new TurboPlane();
tp.fly(); // Plane에 정의/구현된 메소드
tp.boost(); // Turbo에 정의되고 TurboPlane에 구현된 메서드 실행
```

또한 TurboPlane 타입의 객체를 Plane 타입이나 Turbo 타입에 할당하는 것도 가능하다.
```java
TurboPlane tp = new TurboPlane();

Plane p = tp;
p.fly();

Turbo t = tp;
t.boost();
```

## 추상 타입과 유연함
- 추상화(abstraction)는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정이다. 말이 좀 어려운데, 아래의 예를 통해 느낌을 알아보자
    * FTP에서 파일을 다운로드
    * 소켓에서 데이터 읽기
    * DB 테이블의 데이터를 조회
이는 내용을 더 분석해 보니 세 가지 기능은 모두 로그를 수집하기 위한 기능이다. 즉, 각 기능은 로그를 수집하기 위해서 원격 서버에 있는 로그 파일을 조회하는 것이다.
이 경우, 이 세 기능을 추상화하면 __'로그 수집'__ 이라는 개념으로 정의할 수 있다.

```java
class FtpLogFileDownloader {
    
}

class SocketLogReader {
    
}

class DbTableLogGateway {
    
}
```

각각의 기능들을 아래와 같이 추상화할 수 있다.

```java
interface LogCollector {
    public void collect();
}
```

## 추상 타입과 실제 구현의 연결
위의 예제와 같이 설계를 하게 되면 아래와 같이 코드를 만들 수 있다.
```java
// createLogCollector() 는 알맞은 구현 클래스의 객체를 셍상
LogCollect collector = createLogCollector();
collect.collect();
```

- 추상 타입을 사용하는 이유는 아래의 예제를 통해 확인할 수 있다.
```java
public class FlowController {

    // fileName 필드 생략

    public void process() {
        FileDataReader reader = new FileDataReader(fileName); // 객체 생성
        byte[] plainBytes = reader.read(); // 메서드 호출

        ByteEncryptor encryptor = new ByteEncryptor(); // 객체 생성
        byte[] encryptedBytes = encryptor.encrypt(plainBytes); // 메서드 호출

        FilaDateWriter writer = new FileDataWriter(); // 객체 생성
        writer.write(encryptedBytes); // 메서드 호출
    }
}
```
앞서 살펴본 이 예제에서 파일에서 데이터를 읽을 수도 있고 소켓에서 데이터를 읽어오는 기능을 추가할 때 아래와 같이 코드를 짤 수 있다.
```java
public class FlowController {

    private boolean useFile;

    public FlowController(boolean useFile) {
        this.useFile = useFile;
    }

    public void process() {
        byte[] data = null;
        if(useFile) {
            FileDataReader reader = new FileDataReader(); // 객체 생성
            data = fileReader.read(); // 메서드 호출
        } ekse {
            SocketDataReader reader = new SocketDataReader(); // 객체 생성
            data = socketReader.read(); // 메서드 호출
        }

        ByteEncryptor encryptor = new ByteEncryptor(); // 객체 생성
        byte[] encryptedBytes = encryptor.encrypt(plainBytes); // 메서드 호출

        FilaDateWriter writer = new FileDataWriter(); // 객체 생성
        writer.write(encryptedBytes); // 메서드 호출
    }
}
```

이 소스를 보면...앞서 Chapter1, Chapter2 에서 살펴본 지양해야할 소스들이 눈앞을 스쳐갈 것이다.
이제 이 소스에서 공통점을 추출해서 추상화를 진행해보자.

```java
public interface ByteSource { // 어떤 곳으로부터 바이트 데이터 읽기!
    public byte[] read();
}
```

```java
public class FileDataReader implements ByteSource {
    public byte[] read(){

    }
}

public class SocketDataReader implements ByteSource {
    public byte[] read(){

    }
}
```

그리고 데이터를 읽는 객체를 생성하는 책임을 갖는 객체를 하나 추가하자. FlowController 가 data reader들의 객체를 생성하는 책임을 가져야하는 것은 아니기 때문이다.
```java
public class ByteSourceFactory {

    public ByteSource create() {
        if(useFile) {
            return new FileDataReader();
        } else {
            return new SocketDataReader();
        }
    }
    
    private boolean useFile() {
        String useFileVal = System.getProperty("useFile");
        return useFile != null && Boolean.valueOf(useFileVal);
    }

    //sigleton pattern
    public static ByteSourceFactory instance = new ByteSourceFactory();
    public static ByteSourceFactory getintance() {
        return instance;
    }

    private ByteSourceFactory() {

    }
}
```

```java
public class FlowController {

    public void process() {
        
        ByteSouce source = ByteSourceFactory.getInstance().create();
        byte[] data = source.read();

        ByteEncryptor encryptor = new ByteEncryptor(); // 객체 생성
        byte[] encryptedBytes = encryptor.encrypt(plainBytes); // 메서드 호출

        FilaDateWriter writer = new FileDataWriter(); // 객체 생성
        writer.write(encryptedBytes); // 메서드 호출
    }
}
```

이렇게 수정하면 요건이 추가되도 ByteSourceFactory만 변경하면 된다.

## 인터페이스에 대로 프로그래밍하기
- 실제 구현을 제공하는 콘크리트 클래스를 사용해서 프로그래밍 하지 말고 기능을 정의한 인터페이스를 사용해서 프로그래밍하라는 뜻.
- 주의할 점은 타입이 증가하고 구조도 복잡해지기 때문에 모든 곳에서 인터페이스를 사용해서는 안 된다는 것이다. __변화릐 가능성이 높은 경우에 한해서 사용해야한다.__
