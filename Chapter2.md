Chapter2 객체 지향
==================================================================================

- 책에서는 '객체 지향을 잘하려면 이와 대조되는 기법인 절차 지향적으로 프로그램을 작성하면 안된다'라고 한다. 말이 쉽지....하나씩 알아가보자...

## 절차 지향과 객체 지향

1. 절차 지향 : 프로시저로 프로그램을 구성하는 기법을 절차 지향(Procedural Oriented) 프로그래밍이라고 한다. 각 프로시저는 데이터를 사용해서 기능을 구현하며 필요에 따라 다른 프로시저를 사용하기도 한다.
- 시험 성적 관리 프로그램을 예로 들면, 
    - 평균 계산 프로시저는 각 과목의 점수가 보관된 데이터를 읽어서 합을 구한 뒤, 평균 값을 계산한다. 계산된 평균값은 다른 데이터로 생성된다.
    - 화면 출력 프로시저는 평균 계산 프로시저가 생성한 평균 값 데이터와 과목 점수 데이터를 이용해서 화면에 성적을 출력한다.
- 더 프로그램이 커지면 다수의 프로시저들이 데이터를 공유하는 방식으로 만들어지기 때문에, 절차 지향 프로그램은 자연스럽게 데이터를 중심으로 구현하게 된다. 이는 규모가 작을 때는 간단해 보이지만 규모가 커지면 하나의 요건이 추가될 때, 연쇄적으로 프로그램을 수정해야하는 것을 확인할 수 있다.

2. 객체 지향 : 절차 지향과 달리 객체 지향은 데이터 및 데이터와 관련된 프로시저를 객체(Object)로 묶는다. 객체는 프로시저를 실행하는데 필요한 만큼의 데이터를 가지며, 객체들이 모여 프로그램을 구성한다. 객체는 자신만의 데이터와 프로시저를 갖는다. 객체는 자신만의 기능을 제공하며, 각 객체들은 서로 연결되어 다른 객체가 제공하는 기능을 사용할 수 있게 된다. 모든 프로시저가 데이터를 공유하는 절차 지향과는 달리 객체 지향은 객체 별로 데이터와 프로시저를 알맞게 정의해야 한다.


## 객체(Object)

1. 객체를 정의할 때 사용되는 것은 객체가 제공해야할 기능이며, 객체가 내부적으로 어떤 데이터를 갖고 있는 지로는 정의되지 않는다.

- 예를 들어 '소리 크기 제어 객체'가 있다고 가정해보자. 해당 객체의 기능은 아래와 같을 것이다.
    - 소리 크기 증가
    - 소리 크기 감소
    - 음 소거
- 이 객체가 내부적으로 소리 크리를 어떤 데이터 타입 값으로 보관하는지는 중요하지 않다. 또한, 실제로 객체가 어떻게 소리 크기를 증가시키거나 감소시키는지 알 수 없다. 단지, 소리 크기 객체는 '소리 크기 증가', '소리 크기 감소', '음 소거' 라는 __세 가지의 기능__ 을 제공하는 것이 중요할 뿐이다.

2. 인터페이스와 클래스
- 기능들의 집합을 인터페이스라 한다. 이해하기 쉽게 자바의 인터페이스라고 생각하면 쉽다. 인터페이스는 객체를 사용하기 위한 일종의 명세나 규칙이라고 생각하면 쉽다.
- 인터페이스를 구현한 것을 클래스라 한다. 자바의 클래스라고 생각하면 쉽다.

3. 메시지
- 객체 지향은 기능을 제공하는 여러 객체들이 모여서 완성된 어플리케이션을 구성하게 된다.
- 예를 들어, 파일을 읽어 암호화해서 파일에 쓰는 어플리케이션이 있다고 했을 때, 아래와 같이 나눌 수 있다.
    1. 파일 읽기 객체
    2. 암호화 객체
    3. 파일 쓰기 객체

## 객체의 책임과 크기
- 객체는 객체가 제공하는 기능으로 정의된다고 했는데, 이는 다시 말하면 객체마다 자신만의 책임(Responsibility)이 있다는 의미를 갖는다. 예를 들어 앞의 예제는 아래와 같은 책임을 갖는다.
    1. 파일 읽기 객체 - 파일에서 데이터를 읽어와 제공하는 책임
    2. 암호화 객체 - 제공받은 데이터를 암호화해서 다른 파일에 보내는 책임
    3. 파일 쓰기 객체 - 파일에 데이터를 쓰는 책임
한 객체가 갖는 책임을 정의한 것이 바로 타입/인터페이스라고 생각하면 된다.
이렇게 객체의 책임을 결정하는 것이 객체 지향의 출발점이다.
각각의 케이스마다 달라지겠지만 이 책에서 확실한 규칙을 말하는데 이는 __객체가 갖는 책임의 크기가 작을수록 좋다__ 이다.
이와 관련된 원칙이 __단일 책임 원칙(Single Responsibility Principle)__ 이다.

## 의존
- 객체 지향적으로 프로그램을 구현하다 보면, 다른 객체가 제공하는 기능을 이용해서 자신의 기능을 완성하는 객체가 출현하게 된다. 한 객체가 다른 객체를 이용한다는 것은, 실제 구현에서는 한 객체의 코드에서 다른 객체를 생성하거나 다른 객체의 메서드를 호출한다는 것을 뜻한다.
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
이렇게 한 객체가 다른 객체를 생성하거나 다른 객체의 메서드를 호출할 때, 이를 그 객체에 의존(dependency)한다고 표현한다.
위 코드에서는 FlowController가 FileDataReader에 의존한다고 할 수 있다.
__객체를 생성하든 메서드를 호출하든 또는 파라미터로 전달받든 다른 타입에 의존을 한다는 것은 의존하는 타입에 변경이 발생할 때 나도 함께 변경될 가능성이 높다는 것을 뜻한다.__
이렇게 의존이 순환이 되어 순환 의존이 발생하지 않도록 하는 원칙 중의 하나로  __의존 역전 원칙(Dependency inversion principle; DIP)__ 이 있다. (5장에서 자세히)

## 캡슐화
- 객체 지향의 장접은 한 곳의 구현 변경이 다른 곳에 변경을 가하지 않도록 해준다는데 있다. 즉, 수정을 좀 더 원활하게 할 수 있도록 하는 것이 객체 지향적으로 프로그래밍을 하는 이유인 것.   
객체 지향은 기본적으로 __캡슐화__ 를 통해 한 곳의 변화가 다른 곳에 미치는 영향을 최소화 한다.

필자는 아래의 예시를 보고 또 부끄러워졌다. 아래의 예시를 보고...절차 지향 방식의 코드를 지양하자!
```java
public class Member {
    ... // other data
    private Date expiryDate;
    private boolean male;

    public Date getExpireDate() {
        return expiryDate;
    }

    public boolean isMale() {
        return male;
    }
}
```
```java
if(member.gerExpireDate() != null && member.getExpiryDate().getDate() < System.currentTimeMilis()) {
    // 만료되었을 경우 처리
}
```
위와 같은 코드로 만료되었을 경우 판단하게 되면 동일한 코드가 반복되며, 만약 만료일의 조건이 만료 후 30일까지는 유효하게 요견이 변경될 경우 끔찍한 일이 벌어진다.

이를 이제 객체 지향적으로 바꿔보자.
```java
public class Member {
    ... // other data
    private Date expiryDate;
    private boolean male;

    public boolean isExpired() {
        return expiryDate != null && expiryDate.getDate() < System.currentTimeMilis();
    }
}
```

여자의 경우 30일 이후까지 유효한 요건이 추가되었을 경우.
```java
public class Member {
    ... // other data
    private Date expiryDate;
    private boolean male;

    private static final long DAY30 = 1000 * 60 * 60 * 24 * 30; // 30일

    public boolean isExpired() {
        if(male) {
            return expiryDate != null && expiryDate.getDate() < System.currentTimeMilis();
        }
        return expiryDate != null && expiryDate.getDate() < System.currentTimeMilis() - DAY30;
    }
}
```

그럼 이제 어떻게 우리가 익숙해져 있는 절차 지향적 코드에서 객체 지향적 코드로, 캡슐화가 되어 있는 코드로 바꿀 수 있을까?   
아래의 원칙을 지켜보면 습관을 고칠 수 있다고 책에서 말한다.
- Tell, Don't Ask : 데이터를 물어보지 않고 기능을 실행해달라고 말하라는 규칙!
- 데이테르의 법칙(Law of Demeter)
    * 메서드에서 생성한 객체의 메서드만 호출
    * 파라미터로 받은 객체의 메서드만 호출
    * 칠드로 참조하는 객체의 메서드만 호출

```java
/**
 * 
 * 신문 배달부가 고객에게 요금을 받아 가는 상황을 코드로 작성
 *
 */

/**
 * 고객
 */
class Customer {
    private Wallet wallet;

    public Wallet getWallet() {
        return wallet;

    }

}

/**
 * 지갑
 */
class Wallet {
    private int money;

    public int getTotalMoney() {
        return money;

    }

    public void subtractMomey(int debit) {
        money -= debit;

    }

}

/**
 * 신문 배달부
 * 데미테르의 법칙을 어기고 있다.
 * Paperboy 입장에서 customer 객체의 메서드만 사용해야 하는데
 * customer 객체를 통해서 가져온 wallet 객체의 메서드를 호출하고 있다.
 * 
 */
class Paperboy {
    public void takeCharge(Customer customer) {
        int payment = 10000;
        Wallet wallet = customer.getWallet();
        if(wallet.getTotalMoney() >= payment) {
            wallet.subtractMomey(payment);

        } else {
            // 다음에 요금 받으러 오는 처리

        }

    }

}
```

데미테르의 법칙을 지킨다면?
```java
/**
 * 고객
 */
class Customer {
    private Wallet wallet;

    public int getPayment(int payment) {
        if(wallet == null) throw new NotEnoughMoneyException();
        if(wallet.getTotalMoney() >= payment) {
            wallet.subtractMomey(payment);
            return payment;

        }

        throw new NotEnoughMoneyException();

    }

}

/**
 * 지갑
 */
class Wallet {
    private int money;
    public int getTotalMoney() {
        return money;

    }

    public void subtractMomey(int debit) {
        money -= debit;

    }

}

/**
 * 신문 배달부
 * 데미테르 법칙에 따라 customer 객체의 메서드만 호출하고 있다.
 * 데미테르 법칙을 지키기 위해 자연스럽게 Customer 객체의 getPayment() 메서드를 호출하는 것으로 변경하며
 * 비용 지불 기능에 대한 캡슐화를 향상되어 변경에 영향을 받지 않게 된다.
 * 
 */
class Paperboy {
    public void takeCharge(Customer customer) {
        int payment = 10000;
        try {
            int paidAmount = customer.getPayment(payment);
            ...

        } catch (NotEnoughMoneyException ex) {
            // 다음에 요금 받으러 오는 처리

        }

    }

}
```

## 객체 지향 설계 과정
1. 제공해야할 기능을 찾고 또는 세분화하고, 그 기능을 알맞는 객체에 할당한다.
    * 기능을 구현하는데 필요한 데이터를 객체에 추가한다. 객체에 데이터를 먼저 추가하고 그 데이터를 이용하는 기능을 넣을 수도 있다.
    * 기능은 최대한 캡슐화해서 구현한다.
2. 객체 간에 어떻게 메시지를 주고받을 지 결정한다.
3. 과정1과 과정2를 개발하는 동안 지속적으로 반복한다.