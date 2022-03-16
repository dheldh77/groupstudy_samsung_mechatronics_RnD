# 7. 주요 디자인 패턴
## 7. 어댑터 패턴(Adapter) 패턴

![조립을 이용한 어댑터 패턴](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_14/AdapterPattern1.png)

조립을 이용한 어댑터 패턴

![조립을 이용한 어댑터 패턴](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_14/AdapterPattern2.png)

조립을 이용한 어댑터 패턴

- 클라이언트가 요구하는 인터페이스와 재사용하려는 모듈의 인터페이스가 일치하지 않을때 사용할 수 있는 패턴
- 어댑터 클래스의
- 어댑터 클래스는 어댑티 클래스를 클라이언트 인터페이스에 맞춰 주는 책임
- 어댑터 클래스의 메서드는 어댑티 인스턴스를 실행하고 그 결과를 클라이언트 인터페이스에 맞는 리턴 타입으로 변환
- 어댑터 클래스는 클라이언트 인터페이스를 구현하고 있으므로 클라이언트 코드의 수정 없이 변경의 유연함을 얻을 수 있음
- 이로인해 어댑터 패턴은 ‘개방 폐쇄 원칙'을 따를 수 있도록 도와줌

> 조립을 이용한 어댑터 패턴
> 

```java
public class AdapterClass implements ClientInterface {
    private AdapteeClass adapteeClass = new AdapterClass();

    public ConvertedReturn request(String arg) {
        // arg를 AdapteeClass가 요구하는 형식으로 변환
        ConvertedArg convertedArg = new ConvertedArg(arg);
        // AdapteeClass 기능 실행
        Result result = adapteeClass.request(convertedArg);
        // ClientInterface의 리턴 타입으로 변환
        convertedReturn rtn = convert(result);

        return rtn
    }
}
```

> 상속을 이용한 어댑터 패턴
> 

```java
public class AdapterClass extends AdapteeClass implements ClientInterface {
    public ConvertedReturn request(String arg) {
        // arg를 AdapteeClass가 요구하는 형식으로 변환
        ConvertedArg convertedArg = new ConvertedArg(arg);
        // AdapteeClass 기능 실행
        Result result = super.request(convertedArg);
        // ClientInterface의 리턴 타입으로 변환
        convertedReturn rtn = convert(result);

        return rtn
    }
}
```

- 상속을 이용한 어댑터 패턴은 클라이언트가 사용하는 클라이언트 인터페이스가 추상 클래스일 경우, 자바와 같은 클래스 단일 상속을 지원하는 언어에서는 어댑터 구현에 제약

## 8. 옵저버(Observer) 패턴

- 한 객체의 상태 변화를 정해지지 않은 여러 다른 객체에 통지하고 싶을 때 사용되는 패턴
- 옵저버 패턴에는 크게 Subject 객체와 Observer 객체로 구성

![스크린샷 2022-03-14 오후 11.58.37.png](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_14/ObserverPattern.png)

> Suject 객체의 책임
> 
- 옵저버 목록을 관리
- 옵저버를 등록/제거할 수 있는 메서드 제공
- 상태의 변경이 발생하면 등록된 옵저버에 변경 내역을 알림

### 1) 옵저버 패턴 구현하기

> Subject Class
> 

```java
public abstract class StatusSubject {
    private List<StatusObserver> observers = new ArrayList<StatusObserver>();

    public void add(StatusObserver observer) {
        observers.add(observer);
    }

    public void remove(StatusObserver observer) {
        observers.remove(observer);
    }

    // 옵저버 객체의 메서드를 호출하는 방식으로 상태에 변화가 생겼음을 옵저버 객체에게 알림
    public void notifyStatus(Status status) {
        for (StatusObserver observer : observsers)
            observer.onAbnormalStatus(status);
    }
}
```

> 옵저버에게 통지가 필요한 콘크리트 클래스
> 

```java
public class StatusChecker extends StatusSubject {
    public void check() {
        Status status = loadStatus();

        // 특정 상태가 감지되면 상위 클래스의 메서드를 호출해 등록된 옵저버에 상태 값 전달
        if (status.isNotNormal())
            super.notifyStatus(status);
    }

    private Status loadStatus() {

    }
}
```

> Observer Interface
> 

```java
public interface StatusObserver {
    void onAbnormalStatus(Status status);
}
```

- Subject 객체로부터 상태 변화를 전달받을 수 있는 메서드 정의

> Concrete Observer Class
> 

```java
public class StatusEmailSender implements StatusObserver {
    @Override
    public void onAbnormalStatus(Status status) {
        sendEmail(status);
    }

    private void sendEmail(Status status) {
        ...
    }
}
```

- Observer Interface를 상속받아 정의된 메서드 구현
- 콘크리트 클래스에서는 Subject 객체가 호출하는 메서드에서 필요한 기능을 구현

> 옵저버 객체 등록
> 

```java
StatusCheck checker = new StatusChecker();
checker.add(new StatusEmailSender()); // 옵저버 등록
```

- Subject 객체의 상태에 변화가 생길 때 그 내용을 통지받도록 하려면, 옵저버 객체를 주제 객체에 등록
- 옵저버 객체를 등록하면, 특정 상태가 될때마다 옵저버 객체의 메서드를 호출해서 상태 정보를 통지

> 옵저버 패턴의 장점
> 
- 주제 클래스 변경 없이 상태 변경을 통지 받을 옵저버를 추가

### 2) 옵저버 객체에 상태 전달 방법

- 옵저버 객체가 기능을 수행하기 위해 주제 객체의 상태가 필요할 수 있음

> 상태 값만이 필요할 경우
> 

```java
public class FaultStatusSMSSender implements StatusObserver {
    public void onAbnormalStatus(Status status) {
        // 옵저버는 파라미터로 전달받은 상태 값을 사용
        if (status.isFault())
            sendSMS(status);
    }
}
```

- 옵저버 객체는 파라미터로 전달받은 상태값을 확인해서 특정 로직을 수행할 수 있음

> 상태값과 더불어 주제 객체에 접근이 필요할 경우
> 

```java
public class SpecialStatusObserver implements StatusObserver {
    private StatusChecker statusChecker;
    private Siren siren;

    // 생성자를 통해 주체 객체 주입
    public SpecialStatusObserver(StatusChecker statusChecker) {
        this.statusChecker = statusChecker;
    }

    public void onAbnormalStatus(Status stauts) {
        // 파라미터로 전달받은 상태값 확인과 더불어 주체 객체에 접근
        if (status.isFault() && statusChecker.isContinuousFault())
            siren.begin()
    }
}
```

- 상태값 이상의 정보가 필요할 때 옵저버 객체에서 콘크리트 주제 객체에 직접 접근
- 콘크리트 옵저버 클래스에서 콘크리트 주제 클래스로 의존이 발생

### 3) 옵저버에서 주제 객체 구분

- 한 개의 옵저버 객체를 여러 주제 객체에 등록할 경우도 발생할 수 있음
- GUI 프로그래밍에서 흔한 상황
- 이럴 경우, 옵저버 객체에서 각 주제 객체를 구분할 수 있는 방법이 필요

```java
public class MyActivity extends Activity implements View.OnClickListener {
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        ...
        // 두 개의 버튼에 동일한 OnClickListener 객체 등록
        Button loginButton = getViewById(R.id.main_loginbtn);
        loginButton.setOnClickListener(this);
        Button logoutButton = getViewById(R.id.main_logoutbtn);
        logoutButton.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // 주제 객체를 구분할 수 있는 방법 필요
        if (v.getId() == R.id.main_loginbtn) {
            login(id, password);
        } else if (v.getId() == R.id.main_logoutbtn) {
            logout();
        }
    }
}
```

> 주제 객체의 추상 타입
> 
- 위 예제에서는 주제 콘크리트 클래스를 위한 추상 클래스를 정의
- 한 주제에 대한 다양한 구현 클래스가 존재한다면 추상 클래스를 제공함으로써 코드 중복을 방지
- 주제 클래스가 하나 뿐이라면 추상 클래스는 구조만 복잡하게 만들 뿐

### 4) 옵저버 패턴 구현의 고려 사항

> 주제 객체의 통지 기능 실행 주체
> 
- 주제 객체의 상태가 바뀔 때마다 옵저버에게 통지를 해줘야 하는 경우, 주제 객체에서 직접 통지 기능을 실행하는 것이 구현에 유리
- 한 개 이상의 주제 객체의 연속적인 상태 변경 이후에 옵저버에게 통지를 해야하는 경우, 주제 객체의 상태를 변경하는 클라이언트 코드에서 통지 기능을 실행하는 것이 유리

> 옵저버 인터페이스의 분리
> 
- 한 주제 객체가 통지할 수 있는 상태 변경 내역의 종류가 다양한 경우, 각 종류 별로 옵저버 인터페이스를 분리하는 것이 구현에 유리
- 모든 종류의 상태 변화를 수신하는 옵저버 인터페이스가 존재할 경우, 콘크리트 옵저버 클래스는 모든 메서드를 구현해야 함
- 주제 객체 입장에서도 각 상태마다 변경의 이유가 다르기 때문에, 한 개의 옵저버 인터페이스로 관리하는 것은 유연성은 낮춤

> 통지 시점에서의 주제 객체 상태
> 
- 통지 시점에서 주제 객체의 상태에 결함이 없어야 함
- 즉, 옵저버 객체가 완전하지 못한 상태 값을 조회하는 것을 막아야 함
- 해결 방법 중 하나는 상태 변경과 통지 기능에 템플릿 메서드를 적용하는 것

> 옵저버 객체의 실행 제약 조건
> 
- 두 개 이상의 옵저버 객체가 있고, 각 옵저버 객체의 상태 확인/변경 메서드가 많은 리소스를 점유하는 경우 많은 시간을 소비하거나 과부하를 초래할 수 있음
- 따라서, 옵저버 객체의 메서드에 제약을 걸어둠으로써 이러한 상황을 피할 수 있음

> 기타 고려 사항
> 
- 옵저버 객체에서 주제 객체의 상태를 다시 변경하면 어떻게 구현할 것인가..
- 옵저버 자체를 비동기로 실행하는 문제

## 9. 미디에이터(Mediator) 패턴

- 미디에이터 패턴이란 각 객체들이 직접 메시지를 주고받는 대신, 중간에 중계 역할을 수행하는 미디에이터 객체를 두고 미디에이터를 통해서 각 객체들이 간접적으로 메시지를 주고받도록 하는 디자인 패턴

### 1) 미디어 패턴이 적용될 수 있는 경우

![비디오 플레이어 구현](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_14/Mediator1.png)

비디오 플레이어 구현

- 책임에 따라 객체를 분리함으로써 재사용을 높일 수 있을 것이라 기대 했지만, 전체 클래스가 하나의 단일 구조가 되어 변경이나 재사용이 어려운 경우
- 객체 간의 의존이 직접 연결되어 있기 때문
- 객체 간 메시지 흐름을 각 클래스에 직접적인 의존으로 구현하게 되면, 각 클래스의 재사용이 어려워지고 하나의 변경이 연쇄적인 변경을 야기시킴

### 2) 미디어 패턴 적용하기

![미디에이터 패턴 적용](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_14/Mediator2.png)

- VideoMediator가 미디에이터 역할
- 나머지 객체들은 협업 대상(Colleague) 역할을 수행
- 협업 대상 객체들은 서로 직접적인 의존보다 미디에이터 객체를 통해 간접적으로 연결
- 협업 객체는 모든 요청을 미디에이터로 보내며, 미디에이터는 그 요청을 처리할 알맞은 객체를 실행
- 협업 객체가 서로 알 필요 없이 미디에이터가 각 객체 간의 메시지 흐름을 제어하기 때문에, 새로운 협업 객체가 추가되더라도 기존 클래스를 수정할 필요가 없이 미디에이터 객체만 수정하면 됨
- 메시지 흐름이 변경되더라도 미디에이터만 수정함으로써 변경 범위 최소화

> 옵저버 패턴 적용
> 
- 위 예시에서 MediaController에서 미디에이터로의 의존은 ControllerObserver 인터페이스를 이용해 의존을 역전 시킴
- 옵저버 패턴을 적용함으로써 MediaController 클래스는 미디에이터 클래스로의 의존을 제거하고 미디에이터 클래스 없이 MediaController를 재사용할 수 있게 됨

> 미디에이터 패턴의 장점
> 
- 각 협업 클래스에 흩어져 있는 흐름 제어를 미디에이터에 집중 시키기 때문에 협업 클래스의 코드가 단순해짐
- 협업 클래스는 미디에이터에만 의존하거나 또는 미디에이터나 다른 협업 클래스에 의존하지 않기 때문에, 개별 협업 클래스를 수정/확장/재사용이 쉬워짐
- 미디에이터에 각 협업 객체의 흐름 제어 코드가 집중 되어 있기 때문에 전체 협업 객체 간의 메시지 흐름을 이해하고 수정/확장하는 것을 쉽게 만들어 줌

> 미디에이터 패턴의 단점
> 
- 협업 클래스의 개수가 증가할수록 미디에이터의 코드가 복잡
- 미디에이터 자체를 유지보수하는 것은 협업 클래스에 비해 어려워짐

### 1) 추상 미디에이터 클래스의 재사용

- 미디에이터 패턴을 적용할 때 협업 객체 간의 동일한 메시지 흐름이 서로 다른 기능에서 사용될 경우, 미디에이터 추상 클래스를 사용함으로써 재사용을 높일 수 있음
