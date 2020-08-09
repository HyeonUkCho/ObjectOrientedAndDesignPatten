Chapter5 설계원칙: SOLID
==================================================================================
1. 단일 책임 원칙(Single Responsibility Principle; SRP)
2. 개방-페쇄 원칙(Open-Closed Principle; OCP)
3. 리스코프 치환 원칙(Liskov Substitution Principle; LSP)
4. 인터페이스 분리 원칙(Interface Segregation Principle; ISP)
5. 의존 역전 원칙(Dependency Inversion Principle; DIP)

## 단일 책임 원칙(Single Responsibility Principle; SRP)
- 클래스는 단 한 개의 책임을 가져야 한다.
클래스가 여러 책임을 갖게 되면 그 클래스는 각 책임마다 변경되는 이유가 발생하기 때문에, 클래스가 한 개의 이유로만 변경되려면 클래스는 한 개의 책임만을 가져야 한다.

## 개방 폐쇄 원칙(Open-Closed Principle; OCP)
- 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.
이를 좀 더 풀어보면 __기능을 변경하거나 확장할 수 있어야하고, 그 기능을 사용하는 코드는 수정하지 않아야한다__ 이다. 변화하는 부분을 추상화를 한다면 좀 더 쉽게 이 원칙을 지킬 수 있다.
```java
public class ResponseSender {
    private Data data;
    public ResponseSender(Data data) {
        this.data = data;
    }

    public Data getData() {
        return this.data;
    }

    public void send() {
        sendHeader();
        sendBody();
    }

    protected void sendHeader() {
        // send Header Data
    }

    protected void sendBody() {
        // send Body Data
    }
}
```
ResponseSender 클래스의 send() 메서드는 헤더와 바디 내용을 전송하기 위해 sendHeader() 메서드와 sendBody() 메서드를 차례대로 호출하며, 이 두 메서드는 알맞게 HTTP 응답을 생성한다.
그리고 sendHeader() 메서드와 sendBody() 메서드는 protected 공개 범위를 갖고 있기 때문에, 하위 클래스에서 오버라이딩을 할 수 있다.
만약 압축해서 전송하는 기능을 추가하고 싶다면 아래와 같이 소스를 짜면 된다.
```java
public ZippedResponseSender extends ResponseSender {
    public ZippedResponseSender(Data data) {
        super(data);
    }

    @Override
    protected void sendBody() {
        // 데이터 압축 처리
    }
}
```

- 개방 폐쇄 원칙이 깨질 때의 주요 증상
    * 추상화와 다형성을 이용해서 개방 폐쇄 원칙을 구현하기 때문에, 추상화와 다형성이 제대로 지켜지지 않은 코드는 개방 폐쇄 원칙을 어기게 된다. 개방 폐쇄 원칙을 어기는 코드의 전형적인 특징은 다음과 같다.
        1. 다운 캐스팅을 한다.
        예를 들어, 슈팅 게임을 개발하는 경우 플레이어, 적, 미사일 등을 그힌다고 했을 때, 화면에 이들 캐릭터를 표시해 주는 코드가 다음과 같다면 어떨까?
        ```java
        public void drawCharacter(Character character) {
            if(character instanceof Missile) { // 타입 확인
                Missile missile = (Missile) character; // 타입 다운 캐스팅
                missile.drawSpecific();
            } else {
                charactoer.draw();
            }
        }
        ```
        위와 같이 특정 타입인 경우에 별도 처리를 하도록 drawCharacter 메서드를 구현한다면... drawCharacter 메서드는 Character 클래스가 확장될 때 함께 수정된다.
        즉, 변경에는 닫혀 있지 않은 것이다. instanceof 와 같은 타입 확인 연산자가 사용된다면 해당 코드는 개방 폐쇄 원칙을 지키지 않을 가능성이 높다. 이런 경우에는 타입 캐스팅 후 실행하는 메서드가 변화 대상인지 확인해봐야 한다. 예를 들어, 위 코드의 경우 타입이 Missile이면 타입 변환 뒤에 drawSpecific() 메서드를 호출하는데, 이 drawSpecific 메서드가 실제로 객체마다 다르게 동작할 수 있는 변화 대상인지 확인해보는 것이다.

        2. 비슷한 if-else 블록이 존재한다.
        
        


## 리스코프 치환 원칙(Liskov Substitution Principle; LSP)
## 인터페이스 분리 원칙(Interface Segregation Principle; ISP)
## 의존 역전 원칙(Dependency Inversion Principle; DIP)