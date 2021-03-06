# Part1. 객체 지향

<br/>

# Chapter 01. 들어가기

프로젝트가 진행될수록 간단한 요구 사항도 개발하기 힘들거나 변경이 어려운 문제는 소프트웨어가 변화에 대응하기 어렵게 설계되었기 때문이다.

> 지저분해지는 코드
> 
- 조건에 따라 다르게 동작하는 코드를 중첩된 if-else 블록으로 작성하게 되면 요구 사항이 증가할 때마다 코드는 복잡해지고 개발자가 코드를 추가하거나 수정할 위치를 찾는데 점점 오랜 시간을 소요할 것이다.
- 이는 코드를 수정하기 힘들다는 것이고 초기에 새로운 요구 사항을 빠르게 개발했다하더라도, 시간이 지날수록 간단한 요구 사항 조차 개발이 안되는 상황에 직면하게 된다.

> 수정하기 좋은 구조를 가진 코드
> 
- 서로 다른 이유로 변경되는 코드를 관련 코드들로 알맞게 분리한다면 설계 과정에서 다소 시간을 소비하더라도 요구 사항을 빠르게 개발할 수 있는 코드가 될 것이다.

> 객체 지향적으로 코드를 작성했을 때의 이점
> 
- 새로운 기능 추가 시, 기존 코드가 영향을 받지 않음
- 한 기능과 관련된 코드가 한 개의 클래스로 모여 코드 분석/수정이 용이
- 서로 다른 기능에 대한 처리 코드가 섞여 있지 않아 수정 용이
- 즉, 요구 사항이 바뀔 때, 그 변화를 쉽게 적용하고 코드는 유연함을 얻을 수 있음

> 소프트웨어의 가치
> 
- 사용자가 요구하는 기능을 올바르게 제공하는 것
- 요구 사항은 언제나 변할 수 있기 때문에 소프트웨어 또한 변화할 수 있어야 함
- 변화 가능한 유연한 구조를 만들어 주는 핵심 기법 중의 하나가 객체 지향(Object Oriented)
<br/>
<br/>

# Chapter 02. 객체 지향

## 1. 절차 지향과 객체 지향

### 1) 절차 지향

- 데이터를 조작하는 코드를 별도로 분리해서 프로시저와 같은 형태로 만들고, 프로시저로 프로그램을 구성하는 기법
- 각 프로시저는 데이터를 사용해서 기능을 구현하며 필요에 따라 다른 프로시저를 사용
- 여러 프로시저가 동일한 데이터를 공유
- 데이터 중심으로 구현

### 2) 절차 지향 방식으로 프로그래밍 시 문제점

- 데이터 타입이나 의미를 변경해야 할 때, 함께 수정하는 프로시저가 증가
- 같은 데이터를 프로시저들이 서로 다른 의미로 사용

### 3) 객체 지향

- 데이터 및 데이터와 관련된 프로시저를 객체라는 단위로 묶음
- 객체는 프로시저를 실행하는데 필요한 만큼의 데이터를 가지며, 객체들이 모여 프로그램을 구성
- 각 객체는 자신만의 데이터와 프로시저를 가짐
- 프로시저는 자신이 속한 객체의 데이터에만 접근할 수 있으며, 다른 객체에 속한 데이터에는 접근할 수 없음
- 객체의 데이터를 변경하더라도 해당 객체로만 변화가 집중되어 요구 사항의 변화 시 빠르게 적용
- 절차 지향에 비해 설계에 많은 노력이 필요할 수 있지만 코드의 유연성이 증가

## 2. 객체(Object)

### 1) 객체의 핵심 : 기능 제공

- 객체를 정의할 때 사용되는 것은 객체가 제공해야할 기능
- 객체가 내부적으로 어떤 데이터를 갖고 있는지로 정의되는 것이 아님

### 2) 클래스와 인터페이스

- 객체가 제공하는 기능을 오퍼레이션(Operation)이라 부름
- 즉, 객체는 오퍼레이션으로 정의
- 오퍼레이션의 사용법은 시그니처로 구성
- 인터페이스는 객체를 사용하기 위한 일종의 명세
- 클래스는 오퍼레이션을 구현하는데 필요한 데이터 및 오퍼레이션의 구현

> 시그니처(Signature)
> 
- 기능 식별 이름 (함수명)
- 파라미터 및 파라미터 타입 (매개 변수)
- 기능 실행 결과 값 (리턴 타입)

> 인터페이스 (Interface)
> 
- 객체가 제공하는 모든 오퍼레이션 집합
- 서로 다른 인터페이스를 구분할 때 사용되는 명칭이 타입
- 객체가 제공하는 기능에 대한 명세만 제공
- 실제 객체가 기능을 어떻게 구현하는지에 대한 내용은 포함하지 않는다.

> 클래스
> 
- 실제 객체의 구현을 정의

### 3) 메시지 (Massage)

- 오퍼레이션의 실행을 요청(Request)하는 것

## 3. 객체의 책임과 크기

- 객체마다 자신만의 책임(Responsibility)의 가짐
- 한 객체가 갖는 책임을 정의한 것이 타입/인터페이스
- 객체 지향적의 가장 어려우면서 중요한 것이 객체마다 기능(책임)을 할당하는 것
- 모든 상황에 맞는 객체-책임 구성 규칙이 존재하지 않지만, 객체가 갖는 책임의 크기는 작을수록 좋다.
- 객체가 갖는 책임이 클수록 절차 지향에 가까워진다.
- 객체가 갖는 책임이 작아질수록 변경의 유연함을 얻을 수 있음
- 객체 지향의 원칙 중 하나인 단일 책임 원칙을 따를수록 기능의 세부 내용이 변경될 때, 변경이 한 부분에 집중

## 4. 의존 (Dependency)

- 한 객체가 다른 객체를 생성하거나 다른 객체의 메서드를 호출할 때, 그 객체에 의존한다고 표현
- 파라미터로 전달받는 경우도 포함
- 다른 타입에 의존 한다는 것은 의존하는 타입에 변경이 발생할 때 연쇄적으로 변경될 가능성이 높다.
- 의존이 순환해서 발생할 경우 다른 방법이 없는지 고민해야한다.
- 순환 의존이 발생하지 않도록 하는 것이 의존 역전 원칙(DIP, Dependency Inversion Principle)

### 1) 의존의 양면성

```java
public class Autenticator {
	public boolean authenticate(String id, String pwd){
		Member m = findMemberById(id);
		if (m == null) return false;
		
		return m.equalPassword(pwd); // pwd가 m의 암호와 동일하면 True
	}
}

public class AuthenticationHandler {
	public void handlerRequest(String inputId, String InputPwd){
		Authenticator auth = new Authenticator();
		if (auth.authenticate(inputId, inputPwf){
			// 아이디/암호 일치 처리
		} else {
			// 아이디/암호 일치하지 않을 떄 처리
		}
	}
}
```

해당 코드는 간단한 로그인 처리 코드이다. AuthenticationHandler 클래스는 Authenticator 클래스에 의존하고 있기 때문에, Authenticator 클래스에 변경이 생기면 AuthenticationHandler 클래스도 영향을 받는다

```java
public class AuthenticationHandler {
	public void handleRequest(String inputId, String inputPwd){
		Authenticator auth = new Authenticator();
		try {
			auth.authenticate(inputId, inputPwd);
			// 아이디/암호가 일치하는 경우의 처리 
		} catch(MemberNotFoundException ex) {
			// 아이디가 잘못된 경우의 처리
		} catch(InvalidPasswordException ex) {
			// 암호가 잘못된 경우의 처리
		}
	}
}

public class Authenticator {
	public void authenticate(String id, String pwd){
		Member m = findMemeberById(id);
			if (m == null) throw new MemberNotFoudException();
			if (!m.equalPassword(password)) throw new InvalidPasswordException();
	}
}
```

잘못된 아이디를 입력했는지 암호가 틀린 것인지 여부를 로그에 남겨달라는 요구 사항이 생겨났을 때, Authenticator 클래스의 authenticate() 메소드는 단순히 boolean값을 리턴하면 안된다. 따라서, AuthenticationHandler 클래스의 변경 요구로 인해 Authencator 클래스에도 변화가 발생하게 된다. 이는 의존이 상호간의 영향을 준다는 것이다.

> 의존의 양면성
> 
- 한 클래스가 변경되면 클래스를 의존하고 있는 클래스에 영향을 준다.
- 한 클래스의 요구가 변경되면 클래스가 의존하고 있는 타입에 영향을 준다.

## 5. 캡슐화 (Encapsulation)

- 객체 지향은 캡슐화를 통해 한 곳의 변화가 다른 곳에 미치는 영향을 최소화
- 객체가 내부적으로 기능을 어떻게 구현하는지를 감추는 것
- 이를 통해 내부의 기능 구현이 변경되더라도 그 기능을 사용하는 코드는 영향을 받지 않도록 함
- 내부 구현 변경의 유연함을 주는 기법

### 1) 절차 지향 방식 코드

- 절차 지향 방식은 프로시저가 데이터에 직접적으로 접근
- 이로인해 데이터의 변화에 코드들이 직접적인 영향을 받음
- 요구 사항의 변화로 인해 데이터의 구조나 쓰임새가 변경되면 데이터를 사용하는 코드들도 연쇄적으로 수정

### 2) 캡슐화를 위한 규칙

- Tell, Don’t Ask
- 데미테르의 법칙 (Law of Demeter)

> Tell, Don’t Ask
> 
- 데이터를 물어보지 않고, 기능을 실행해 달라고 말하는 규칙
- 기능 실행을 요청하는 방식으로 코드를 작성하다 보면, 자연스럽게 해당 기능을 어떻게 구현했는지 여부가 감춰짐

> 데미테르의 법칙 (Law of Demeter)
> 
- 메서드에서 생성한 객체의 메서드만 호출
- 파라미터로 받은 객체의 메서드만 호출
- 필드로 참조하는 객체의 메서드만 호출

[신문 배달부와 지갑](https://smjeon.dev/etc/law-of-demeter/)

## 6. 객체 지향 설계 과정

> 객체 지향 설계 과정
> 
1. 제공해야 할 기능을 찾고 또는 세분화하고, 그 기능을 알맞는 객체에 할당
a. 기능을 구현하는데 필요한 데이터를 객체에 추가. 객체에 데이터를 먼저 추가하고 기능을 넣을 수 있음
b. 기능을 최대한 캡슐화
2. 객체 간에 어떻게 메시지를 주고 받을 지 결정
3. 1번, 2번 과정을 개발하는 동안 지속적으로 반복

> 객체 설계
> 
- 객체의 크기는 한 번에 완성되기 보다는 구현을 진행하는 과정에서 점진적으로 명확
- 구현 과정에서 한 클래스에 여러 책임이 섞여 있다는 것을 알게 되면, 객체를 새로 만들어서 책임을 분리
- 최초에 만든 설계가 완벽하지 않으며, 개발이 진행되면서 설계도 함께 변경
- 설계를 할 때에는 변경되는 부분을 고려한 유연한 구조를 갖도록 노력

# Chapter 03. 다형성과 추상 타입

- 객체 지향의 장점은 구현 변경의 유연함
- 유연함을 얻을 수 있는 또 다른 방법은 ‘추상화’
- ‘추상화’를 가능케 해주는 것이 ‘다형성’

## 1. 상속 (Inheritance)

- 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법
- 상속 대상이 되는 클래스를 상위 클래스(super class) 또는 부모 클래스(parent class)라고 함
- 상속을 받는 클래스를 하위 클래스(sub class) 또는 자식 클래스(child class)라고 함
- 하위 클래스는 상위 클래스에 정의된 구현을 물려받음
- 많은 언어에서 private 범위를 갖는 메서드나 필드를 제외한 나머지를 물려받을 수 있도록 하고 있음
- 하위 클래스는 필요에 따라 상위 클래스에 정의된 메서드를 재정의 할 수 있는데 이를 오버라이딩(Overriding)이라고 함

## 2. 다형성과 상속

- 다형성(Polymorphism)은 한 객체가 여러 가지(poly) 모습(morph)을 갖는 것
- 즉, 한 객체가 여러 타입을 가질 수 있다는 것
- 상속을 통해 다형성을 구현

### 1) 인터페이스 상속과 구현 상속

- 다형성을 구현하기 위해 타입을 상속받는 방법을 사용하는데 타입 상속은 크게 ‘인터페이스 상속'과 ‘구현 상속'으로 구분

> 인터페이스 상속
> 
- ‘인터페이스 상속'은 순전인 타입 정의만을 상속
- 자바와 같이 클래스 다중 상속을 지원하지 않는 언어에서는 인터페이스를 이용해 객체가 다형성을 가짐

> 구현 상속
> 
- ‘구현 상속'은 클래스 상속을 통해서 이루어짐
- ‘구현 상속'은 보통 상위 클래스에 정의된 기능을 재사용하기 위한 목적
- 클래스 상속은 구현을 재사용하면서 다형성도 함께 제공

## 3. 추상 타입과 유연함

- ‘추상화(abstraction)’는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정
- 추상화된 타입은 오퍼레이션의 시그니처만 정의할 뿐 실제 구현을 제공하지는 못함(의미만 제공)

### 1) 추상 타입과 실제 구현의 연결

- 추상 타입과 실제 구현 클래스는 상속을 통해 연결
- 즉, 구현 클래스가 추상 타입을 상속받는 방법으로 연결
- 실제 구현을 제공하는 클래스는 콘크리트 클래스(Concrete class)라고 함

### 2) 추상 타입을 이용한 구현 교체의 유연함

- 예시를 통해 추상 타입을 이용한 구현 교체의 유연함을 알아보자

```java
public class FlowController {
    public void process(){
        // 파일로부터 데이터를 읽어옴
        FileDataReader reader = new FileDataReader();
        byte[] data = reader.read();

        // 암호화
        Encryptor encryptor = new Encryptor();
        byte[] encrpyptedData = encryptor.encrpyt(data);

        // 파일에 쓰기
        FileDataWriter writer = new FileDataWriter();
        writer.write(encrpyptedData)
    }
}
```

위 코드는 파일로부터 데이터를 읽어들어와 암호화 후 파일에 저장하는 프로그램이다. 해당 기능의 초기 요구 사항은 파일로부터 데이터를 읽어들이는 것이었지만 요구 사항이 소켓으로 데이터를 읽어들어올 수도 있다는 것이 추가 되었다면 코드는 아래와 같이 변할 것이다.

```java
public class FlowController {
    private boolean useFile;

    public FlowController(boolean useFile){
        this.useFile = useFile;
    }

    public void process(){
        byte[] data = null;

        // 파일 또는 소켓으로부터 데이터를 읽어옴
        if (useFile){
            FileDataReader fileReader = new FileDataReader();
            byte[] data = fileReader.read();
        } else {
            SocketDataReader socketReader = new SocketDataReader();
            byte[] data = socketReader.read();
        }

        // 암호화
        Encryptor encryptor = new Encryptor();
        byte[] encrpyptedData = encryptor.encrpyt(data);

        // 파일에 쓰기
        FileDataWriter writer = new FileDataWriter();
        writer.write(encrpyptedData)
    }
}
```

요구 사항의 변경으로 인해 비슷한 if-else 블록이 추가가 되었다. 추상화를 통해 제거해보자

```java
public interface ByteSource {
    public byte[] read();
    ...
}

public class FileDataReader implements ByteSource {
    public byte[] read() {
        ...
    }
}

public class SocketDataReader implements ByteSource {
    public byte[] read() {
        ...
    }
}

public class FlowController {
    private boolean useFile;

    public FlowController(boolean useFile){
        this.useFile = useFile;
    }

    public void process(){
        ByteSource source = null;

        // 파일 또는 소켓으로부터 데이터를 읽어들이는 인스턴스 생성
        if (useFile) source = new FileDataReader();
        else source = new SocketDataReader();

        // 데이터를 읽어들어옴
        byte[] data = source.read();

        // 암호화
        Encryptor encryptor = new Encryptor();
        byte[] encrpyptedData = encryptor.encrpyt(data);

        // 파일에 쓰기
        FileDataWriter writer = new FileDataWriter();
        writer.write(encrpyptedData)
    }
}
```

파일 또는 소켓으로부터 데이터를 읽어들이는 책임을 ‘읽어들인다'라는 기능으로 ByteSource라는 인터페이스로 추상화하고 FlowController에서는 생성된 인스턴스와 관계없이 read() 함수를 실행할 수 있다.

하지만, 콘크리트 클래스를 이용해 객체를 생성하는 부분이 if-else 블록으로 여전히 남아있기 때문에 해당 기능과 관련되어 요구 사항이 추가될수록 코드는 복잡해질 것이다.

이 부분은 객체를 생성하는 책임을 별도의 클래스로 분리하게 되면 FlowController 클래스는 ‘~에서 읽어들인다’라는 기능의 요구사항이 변경되더라도 코드의 변경이 없을 것이다.

```java
public class FlowControlle {
    public void process(){
        // 인스턴스를 생성하는 책임을 ByteSourceFactory 클래스에서 구현
        ByteSource source = ByteSourceFactory.getInstance().create();
        // 데이터를 읽어들어옴
        byte[] data = source.read();

        // 암호화
        Encryptor encryptor = new Encryptor();
        byte[] encrpyptedData = encryptor.encrpyt(data);

        // 파일에 쓰기
        FileDataWriter writer = new FileDataWriter();
        writer.write(encrpyptedData)
    }
}
```

위 예제를 통해 추상 타입을 이용한 구현 교체의 유연함을 확인해 보았다. 추상화는 공통된 개념을 도출해서 추상 타입을 정의해 주기도 하지만, 많은 책임을 가진 객체로부터 책임을 분리할 수 있게 해준다.

### 3) 변화되는 부분을 추상화하기

- 추상화를 잘 하려면 다양한 상황에서 코드를 작성하고 과정에서 유연한 설계를 만들어 보는 경험이 필요
- 변화될 부분을 미리 예측해서 추상화하는 것은 어려운 일
- 그렇기 때문에 변화되는 부분을 추상화하는 것부터 시작
- 요구 사항이 바뀔 때 변화되는 부분은 이 후에도 변경될 가능성이 많기 때문에 추상화한다면 유연하게 대처 가능
- 추상화를 하면 추상 타입을 사용하는 코드에 영향을 주지 않으면서 추상 타입의 실제 구현을 변경할 수 있음

### 4) 인터페이스에 대고 프로그래밍하기

- 실제 구현을 제공하는 콘크리트 클래스를 사용해서 프로그래밍하지 않고 기능을 정의한 인터페이스를 사용해 프로그래밍할 것
- 인터페이스는 최초 설계에서 바로 도출되기 보다는, 요구 사항의 변화와 함께 점진적으로 도출
- 즉, 인터페이스는 새롭게 발견된 추상 개념을 통해서 도출
- 추상 타입을 사용하면 기존 코드를 건드리지 않으면서 콘크리트 클래스를 교체할 수 있음
- ‘인터페이스에 대고 프로그래밍하기'는 추상화를 통한 유연함을 얻기 위한 규칙

> 추상화의 유의점
> 
- 추상화를 통해 유연함을 얻는 과정에서 타입이 증가하고 구조가 복잡해지기 때문에 모든 곳에서 인터페이스를 사용하면 안됨
- 과도한 추상화는 불필요하게 프로그램의 복잡도만 증가시킴
- 인터페이스를 사용할 때는 ‘변화 가능성이 높은' 경우에만 적용

### 5) 인터페이스는 인터페이스 사용자 입장에서 만들기

- 인터페이스를 작성할 때에는 그 인터페이스를 사용하는 코드입장에서 작성

```java
public class FlowControlle {
    public void process(){
        // 인스턴스를 생성하는 책임을 ByteSourceFactory 클래스에서 구현
        ByteSource source = ByteSourceFactory.getInstance().create();
        // 데이터를 읽어들어옴
        byte[] data = source.read();

        // 암호화
        Encryptor encryptor = new Encryptor();
        byte[] encrpyptedData = encryptor.encrpyt(data);

        // 파일에 쓰기
        FileDataWriter writer = new FileDataWriter();
        writer.write(encrpyptedData)
    }
}
```

- 위 코드 예시에서 인터페이스 명을 ‘ByteSource’라고 정의했는데 ‘FileDataReaderIF’라고 명명했을 때, FlowController 입장에서는 해당 인터페이스를 상속받은 클래스는 ‘데이터를 읽어들인다'라는 의미보다는 ‘파일에서 데이터를 읽어들인다'라는 의미로 해석
- 따라서, 인터페이스를 의존하는 클래스의 입장에서 인터페이스의 이름을 지을 것

### 6) 인터페이스와 테스트

- 테스트 코드를 작성할 때, 실제 콘크리트 클래스 대신에 진짜처럼 행동하는 객체를 Mock(가짜, 모의) 객체라 함
- Mock 객체를 사용함으로써 실제 사용할 콘크리트 클래스의 구현 없이 테스트할 수 있음
- Mock 객체를 만드는 방법은 다양하게 존재하지만, 사용할 대상을 인터페이스로 추상화하면 좀 더 쉽게 Mock 객체를 만들 수 있게 됨
- 이는 사용할 코드의 완성을 기다릴 필요 없이 내가 만든 코드를 먼저 빠르게 테스트할 수 있도록 해줌

# Chapter 04. 재사용: 상속보단 조립

## 1. 상속과 재사용

- 상속은 기능을 확장할 수 있기 때문에 기능의 재사용이라는 장점이 있지만, 변경의 유연함이라는 측면에서 치명적인 단점을 가짐

> 상속을 통한 재사용의 단점
> 

(1) 상위 클래스 변경의 어려움

- 어떤 클래스를 상속받는 다는 것은 그 클래스를 의존하는 것.
- 의존하는 클래스의 코드가 변경되면 영향을 받을수 있음
- 상속 계층을 따라 상위 클래스의 변경이 하위 클래스에 영향을 준다.

(2) 클래스의 불필요한 증가

- 필요한 기능의 조합이 증가할수록 클래스의 개수는 함께 증가하고 구조가 복잡해진다.

(3) 상속의 오용

- 상속은 IS-A 관계가 성립할 때에만 사용해야 한다.
- 서로 다른 책임을 갖고 있는 클래스의 구현을 재사용하기 위해 상속을 받게 되면 잘못된 사용으로 인한 문제점이 발생한다.

## 2. 조립의 이용한 재사용

- 객체 조립(Composition)은 여러 객체를 묶어서 더 복잡한 기능을 제공하는 객체를 만들어 내는 것
- 객체 지향 언어에서 객체 조립은 보통 필드에서 다른 객체를 참조하는 방식으로 구현
- 한 객체가 다른 객체를 조립해서 필드로 갖는다는 것은 다른 객체의 기능을 사용한다는 의미를 내포
- 조립을 사용하면 상속을 잘못 사용해서 발생했던 문제가 제거(상위 클래스 변경의 어려움, 클래스의 불필요한 증가, 상속의 오용)
- 또한 조립은 런타임에 조립 대상 객체를 교체할 수 있음

> 상속보다는 객체 조립을 사용할 것
> 
- 상속을 사용하다 보면 변경의 관점에서 유연함이 떨어질 가능성이 높기 때문에 객체 조립을 먼저 고민할 것
- 단, 조립의 단점은 런타임 구조가 복잡해지고 구현이 어렵다는 것

## 3. ****위임(Delegation)****

- 책임을 다른 객체에 넘긴다는 의미로 조립 방식을 이용해 위임을 구현
- 위임은 조립과 마찬가지로 요청을 위임할 객체를 필드로 연결
- 또는 메소드 안에서 객체를 새로 생성해서 요청을 전달해도 위임이란 의미에서 벗어나지 않음

## 4. ****상속을 사용해야할 때****

- 재사용이라는 관점이 아닌 기능의 확장이라는 관점에 적용
- 명확한 IS-A 관계가 성립할 때 점진적으로 상위 클래스의 기능을 확장해나가기위해 적용
