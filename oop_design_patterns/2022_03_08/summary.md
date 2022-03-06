# Chapter 07. 주요 디자인 패턴

- 객체 지향 설계는 소프트웨어로 해결하고 하는 문제를 다루면서, 동시에 재설계를 최소화하면서 요구 사항의 변화를 수용할 수 있도록 만들어야 한다.
- 반복적으로 사용되는 설계는 클래스, 객체의 구성, 객체 간 메시지 흐름에서 일정 패턴을 갖는다.
- 이렇게 자주 발생하는 문제들을 해결할 수 있도록 정의되어 있는 설계/패턴을 디자인 패턴이라 한다.

> 디자인 패턴 적용의 장점
> 
- 상황에 맞는 올바른 설계를 빠르게 적용
- 각 패턴의 장단점을 통해 설계를 선택하는데 도움
- 설계 패턴에 이름을 붙임으로써 시스템의 문서화, 이해, 유지 보수에 도움

## 1. 전략(Strategy) 패턴

> 예제
> 

```java
public class Calculator {
    public int calculator(boolean firstGuest, List<Item> items) {
        int sum = 0;
        for (Item item : items) {
            if (firstGuest)
                sum += (int) (item.getPrice() * 0.9)
            else if (! item.isFresh())
                sum += (int) (item.getPrice() * 0.8)
            else
                sum += item.getPrice()
        }
        return sum;
    }
}
```

- 위 코드는 첫번째 손님인지 또는 상품의 신선도 여부에 따라 다른 가격 정책을 사용하고 있다.

> 문제점
> 
- 서로 다른 계산 정책들이 한 코드에 섞여, 정책이 추가될수록 코드 분석을 어렵게 만든다.
- 정책이 추가될 때마다 메서드를 수정하는 것이 어려워지고 if-else 블록이 추가된다.

### 1) Strategy Pattern 적용하기

> 전략 패턴 적용
> 

![Strategy Pattern](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/StrategyPattern.png)

- 인터페이스(Strategy)로 계산 정책을 추상화
- 콘크리트 클래스(Strategy Concrete)에서 상황에 맞는 계산 정책을 제공
- 기능 자체에 대한 책임을 갖고 있는 클래스를 Context라 함
- 컨텍스트는 사용할 전략을 직접 선택하는 대신 클라이언트가 컨테스트에 사용할 전략을 전달(DI)

> Context Class
> 

```java
// Context
public class calculator {
    private DiscountStrategy discountStrategy;

    // 생성자를 통한 의존 주입
    public Calculator(DiscocuntStrategy discountStrategy) {
        this.discountStrategy = discountStrategy;
    }

    public int calculator(List<Item> items) {
        int sum = 0;
        for (Item item : items) {
            sum += discountStrategy.getDiscountPrice(item);
        }
        return sum;
    }
}
```

- Context 클래스는 Strategy Interface 타입의 멤버 변수를 갖는다.
- Context 클래스는 생성자를 통해서 사용할 전략 객체를 주입받는다.
- Context 클래스는 주입 받은 전략 객체를 메서드를 통해 사용한다.

> Strategy Interface
> 

```java
// Strategy Interface
public interface DiscountStrategy {
    int getDiscountPrice(Item itme);
}
```

- Strategy Interface 에서는 Context에서 사용할 메서드의 정의를 담당한다.
- 구현은 Strategy Interface를 상속받는 하위 클래스에서 한다.

> Strategy Concrete Class
> 

```java
// Strategy Concrete Class
public class FisrtGuestDiscountStrategy implements DiscountStrategy {
    @Override
    public int getDiscountPrice(Item itme) {
        return (int) (item.getPrice() * 0.9);
    }
}
```

- Strategy Interface를 상속받는 하위 클래스에서 메서드 정책을 구현한다.

> Client Code
> 

```java
// client code 
public class testStrategy {
    private DiscountStrategy strategy;

    public void onFirstGuestButtonClick() {
        strategy = new FisrtGuestDiscountStrategy();
    }

    public void onCalculationButtonClick() {
        Calculator cal = new Calculator(strategy);
        int price = cal.calcaulate(items);
    }
}
```

- 전략 객체는 Context를 사용하는 클라이언트에서 직접 생성
- 컨텍스트를 사용하는 클라이언트가 전략의 상세 구현에 대한 의존이 발생
- 컨텍스트의 클라이언트가 전략의 인터페이스가 아닌 상세 구현을 안다는 것이 문제처럼 보일 수 있으나, 전략의 콘크리트 클래스와 클라이언트의 코드가 쌍을 이루기 때문에 유지 보수 문제가 발생할 가능성이 줄어듦
- 클라이언트 코드에서 전략 객체를 직접 생성하는 것은 오히려 코드 이해를 높이고 코드 응집을 높여주는 효과

### 2) 전략 패턴의 이점

- 컨텍스트 코드의 변경 없이 새로운 전략을 추가( 클라이언트 코드에 객체를 생성해 주기만 하면 된다.)
- 전략 패턴을 적용함으로써 확장에는 열려 있고 변경에는 닫혀 있음 → 개방 폐쇄 원칙을 따르는 구조

### 3) 전략 패턴을 적용할 수 있는 경우

- if-else로 구성된 코드 블록이 비슷한 알고리즘을 수행하는 경우 전략 패턴을 적용함으로써 코드를 확장 가능하도록 변경할 수 있음
- 완전히 동일한 기능을 제공하지만 성능의 장단점에 따라 알고리즘을 선택해야하는 경우에 사용할 수 있음

## 3. 템플릿 메서드(Template Method) 패턴

### 1) 템플릿 메서드 패턴을 적용할 수 있는 경우

- 일부 과정의 구현만 다르고 완전히 동일한 절차를 가진 코드를 작성하게 될 때 적용할 수 있는 패턴
- 즉, 샐행 과정/단계는 동일한데 각 단계 중 일부의 구현이 다른 경우에 적용

### 2) 템플릿 메서드 패턴 적용하기

- 템플릿 메서드 패턴은 ‘실행 과정을 구현한 상위 클래스'와 ‘실행 과정의 일부 단계를 구현한 하위 클래스'로 구현할 수 있다.
- ‘상위 클래스’는 실행 과정을 구현한 메서드를 제공
- ‘상위 클래스'에서 기능을 구현하는데 필요한 각 단계를 정의
- 일부 단계는 ‘상위 클래스’의 추상 메서드를 호출하는 방식으로 구현
- 추상 메서드는 구현이 다른 단계에 해당

> 템플릿 메서드 : 상위 클래스
> 

```java
public abstract Authenticator {
    // template method
    public Auth authenticate(String id, String pw) {
        if ( ! doAuthenticate(id, pw) )
            throw createException();

        return createAuth(id);
    }

    protected abstract boolean doAuthenticate(String id, String pw);

    private RuntimeException createException() {
        throw new AuthException();
    }

    protected abstract Auth createAuth(String id);
}
```

- 템플릿 메서드에서 동일한 실행 과정을 구현
- 일부의 구현이 다른 경우를 별도의 추상 메서드로 분리
- 하위 클래스에서 템플릿 메서드에서 호출하는 다른 메서드만 재정의

> 템플릿 메서드 : 하위 클래스
> 

```java
public class LdapAuthenticator extends Authenticator {
    @Override
    protected boolean doAuthenticate(String id, String pw) {
        return ldapCilent.authenticate(id, pw);
    }

    @Override
    protected Auth createAuth(String id) {
        LdapContext ctx = ldpaCilend.find(id);
        return new Auth(id, ctx.getAttribute("name"));
    }
}
```

- 템플릿 메서드를 구현한 상위 클래스를 상속받는 하위 클래스는 일부 과정의 구현만을 제공
- 전체 실행 과정은 상위 클래스의 템플릿 메서드를 사용

### 3) 템플릿 메서드의 이점

- 동일한 실행 과정의 구현을 제공하면서 동시에 하위 클르새에서 일부 단계를 구현함으로써 코드가 중복되는 것을 방지
- 즉, 코드 중복을 제거하고 코드 재사용을 제공

> 상위 클래스가 흐름 제어의 주체
> 
- 상위 타입의 템플릿 메서드가 모든 실행 흐름을 제어
- 하위 타입의 메서드는 템플릿 메서드에서 호출되는 구조
- 템플릿 메서드의 경우 외부에서 제공하는 기능에 해당되기 때문에 public
- 일부 구현이 다른 코드는 하위 타입에서 재정의할 수 있어야 하기 때문에 protected
- 템플릿 메서드에서 호출하는 메서드를 ‘추상 메서드'가 아닌 기능의 확장을 위해 기본 구현을 제공할 수 도 있음(훅 메서드)

> 훅(hook) 메서드
> 
- 상위 클래스에서 실행 시점이 제어되고, 기본 구현을 제공하면서, 하위 클래스에서 알맞게 확장할 수 있는 메서드

### 4) 템플릿 메서드와 전략 패턴의 조합

- 템플릿 메서드와 전략 패턴을 함께 사용하면 상속이 아닌 조립의 방식으로 템플릿 메서드 패턴을 활용 가능
- 대표적인 예가 스프링 프레임워크의 Template으로 끝나는 클래스
- 템플릿 메서드를 실행할 때, 변경되는 부분을 실행할 객체를 ‘파라미터’를 통해 전달받는 방식

> 템플릿 메서드
> 

```java
// 트랜잭션의 시작/커밋/롤백 등의 실행 흐름을 제공하는 템플릿 메서드
public <T> T execute (TransactionCallback<T> action) throws TransactionException {
    // 일부 코드 생략
    TransactionStatus status = this.transcationManager.getTransaction(this);
    T result;

    try {
        // 파라미터로 전달받는 객체의 메서드를 호출
        result = action.doInTransaction(status);
    }
    catch (RuntimeException ex) {
        rollbackOnException(status, ex);
        throw ex;
    }
    ... // 기타 다른 익셉션 처리 코드
    this.transactionManager.commit(status);
    return result;
}
```

- 템플릿 메서드가 파라미터로 전달받은 객체의 메서드를 호출하고 있음

> 클라이언트 코드
> 

```java
transactionTemplate.execute(new TransactionCallback<String() {
    public String doInTransaction(TransactionStatus status) {
        // 트랜잭션 범위 안에서 실행될 코드
    }
});
```

- 위 예제의 템플릿 메서드를 호출하면서 원하는 기능을 구현한 객체를 인자로 넣어주고 있음

> 조립의 방식으로 템플릿 메서드를 사용할 때 이점
> 
- 상속에 기반을 둔 템플릿 메서드 구현과 비교해서 유연함
- 런타임에 템플릿 메서드에서 사용할 객체를 교체할 수 있다는 장점

> 조립의 방식으로 템플릿 메서드를 사용할 때 단점
> 
- 훅 메서드와 같은 확장의 기능을 구현하기 복잡

## 4. 상태(State) 패턴

> 예제
> 

```java
public class VendingMachine {
    public static enum State { NOCOIN, SELECTABLE, SOLDOUT }

    private State state = State.NOCOIN;

    public void insertCoind(int coin) {
        swtich(state) {
        case NOCOIN:
            increaseCoin(coin);
            state = State.SELECTABLE;
            break;
        case SELECTABLE:
            increaseCoin(coin);
            break;
        case SOLDOUT:
            returnCoin();
        }
    }

    public void select(int productId) {
        switch(state) { 
        case NOCOIN:
            //do not anything
            break;
        case SELECTABLE:
            provideProduct(productId);
            decreaseCoin();
            if (hasNoCoin())
                state = State.NOCOIN;
        case SOLDOUT:
            // do not anything
        }
    }
}
```

- 위 코드는 코인 상태에 따라 다른 동작을 하는 자판기 클래스

> 문제점
> 
- 상태가 많아질수록 복잡해지는 조건문이 여러 코드에서 중복해서 출현
- 코드 변경을 어렵게 만듬
- “상태"에 따라 동일한 기능 요청의 처리를 다르게 해야함

### 2) 상태 패턴 적용하기

![State Pattern](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/StatePattern.png)

- 상태를 타입으로 분리하고, 각 상태 별로 알맞은 하위 타입을 구현
- ‘상태 객체가 기능을 제공’
- 상태(State) 인터페이스는 모든 상태가 동일하게 적용 되는 기능을 명세
- Context는 필드로 상태 객체를 가지며, 클라이언트로부터 기능 실행 요청을 받으면 상태 객체에 처리를 위임

> Context
> 

```java
public class VendingMachine {
    private State state;

    public VendingMachine() {
        state = new NoCoinState();
    }

    public void insertCoing(int coin) {
        state.increaseCoin(coin, this); // 상태 객체에 위임
    }

    public void select(int productId) {
        state.select(productId, this); // 상태 객체에 위임
    }

    public void changeState(State newState) {
        this.state = newState;
    }
}
```

- Context 클래스는 State 타입의 멤버 변수에 기능 실행의 책임을 위임
- Context 클래스에 상태 객체 변경 메서드가 있는 것을 확인할 수 있음

> State Interface
> 

```java
public interface State {
    void increaseCoint(int coin, VendingMachine vm);
    void select(int productId, VendingMachine vm);
}
```

- State 인터페이스에는 모든 상태에 동일하게 적용되는 기능에 대한 명세가 있음

> State Class
> 

```java
public class NoCoinState implements State {
    @Override
    public void increaseCoin(int coin, VendingMachine vm) {
        vm.increaseCoin(coin);
        vm.changeState(new SelectableState()); // 상태 변경
    }

    @Override
    public void select(int productId, VendingMachine vm) {
        SoundUtil.beep();
    }
}

public class SelectableState implements State {
    @Override
    public void increaseCoin(int coin, VendingMachine vm) {
        vm.increaseCoin(coin);
    }

    @Override
    public void select(int productId, VendingMachine vm) {
        vm.provoideProduct(productId);
        vm.decreaseCoin();

        if (vm.hasNoCoin())
            vm.changeState(new NoCoinState());
    }
}
```

- 상태 클래스를 구현함으로써 기존 Context 클래스에 구현되어 있던 상태 별 동작 구현 코드가 각 상태의 구현 클래스로 이동
- Context 클래스는 상태 객체에 기능 수행을 위임하므로써 단순해짐

### 3) 상태 패턴의 이점

- 새로운 상태가 추가되더라도 컨텍스트 코드가 받는 영향은 최소화
- 상태에 따른 동작을 구현한 코드가 각 상태별로 구분되기 때문에 상태 별 동작을 수정하기가 쉬움

### 4) 상태 변경의 주체

- 위 코드 예제에서는 각 상태 객체에서 컨텍스트의 메서드를 이용해서 상태를 변경하고 있음
- 상태 변경을 할 수 있는 주체는 컨텍스트와 상태 클래스가 있는데, 어떤 경우에 누가 해야할까?

> Context에서 상태를 변경하는 방식
> 
- 비교적 상태 개수가 적고 상태 변경 규칙이 거의 바뀌지 않는 경우

> 상태 객체에서 Context의 상태를 변경하는 방식
> 
- 컨텍스트에 영향을 주지 않으면서 상태를 추가하거나 상태 변경 규칙을 바꿀 수 있게 됨
- 상태 구현 클래스가 많아질수록 상태 변경 규칙을 파악하기 어려워진다.
- 또한, 한 상태 클래스에서 다른 상태 클래스에 대한 의존이 발생할 수 도 있다.

## 5. 데코레이터(Decorator) 패턴

- 상속은 기능을 확장하는 방법을 제공하지만, 다양한 조합의 기능 확장이 요구될 때 클래스가 불필요하게 증가되는 문제가 발생
- 이럴 경우 사용할 수 있는 패턴이 ‘데코레이터(Decorator) 패턴’
- 데코레이터 패턴은 위임을 하는 방식으로 기능 확장

### 1) 데코레이터 패턴 적용하기

> 데코레이터 패턴 설계
> 

![Decorator Pattern](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/DecoratorPattern.png)

- 인터페이스는 기존 기능을 정의
- Implementation 클래스는 인터페이스에서 정의한 기능을 구현
- 기능 확장을 위해 Implementation 클래스를 상속받지 않고 Decorator라는 별도의 추상 클래스를 사용
- Decorator 클래스는 모든 데코레이터를 위한 기반 기능을 제공하는 추상 클래스
- Decorator 클래스의 메서드는 생성자를 통해 전달받은 Implementation 객체에 책임을 위임

> Decorator 클래스
> 

```java
public abstract class Decorator implements FileOut {
    private FileOut delegate; // 위임 대상

    // 생성자를 통해 위임 대상 주입
    public Decorator(FileOut delegate) {
        this.delegate = delegate;
    }

    protected void doDelegate(byte[] date) {
        delegate.write(data); // delegate에 책임 위임
    }
}
```

- 데코레이터 클래스는 생성자를 통해 위임 대상을 주입받는다.
- 데코레이터 클래스의 메서드를 통해 주입받은 객체에 책임을 위임한다.

> Dacorator 클래스 - 하위 클래스 구현
> 

```java
public class EncrpytionOut extends Decorator {
    // 위임 대상 주입
    public EncrpytionOut(FileOut delegate) {
        super(delegate);
    }

    public void write(Byte[] data) {
        byte[] encrpytedData = encrpyt(data);
        super.doDelegate(encrpytedData); // delegate에 책임 위임
    }
}
```

- 데코레이터 클래스를 상속받은 하위 클래스는 자신의 기능을 수행한 두이 상위 클래스의 메서드를 이용해 위임 객체에 전달

### 2) 데코레이터 패턴의 이점

- 데코레이터를 조합하는 방식으로 기능을 확장할 수 있음

```java
// 기능 조합
FileOut delegate = enw FileOutImpl();
FileOut fileOut = new EncrpytionOut(new ZipOut(delegate));
fileOut.write(data);

// 순서 변경
// 압축 -> 압호화
FileOut fileOut = new EncrpytionOut(new ZipOut(delegate));
// 암호화 -> 압축
FileOut fileOut = new ZipOut(new EncrpytionOut(delegate));
```

- 각 확장 기능들의 구현이 별도의 클래스로 분리되기 때문에, 각 확장 기능 및 원래 기능을 서로 영향 없이 변경할 수 있도록 만들어 줌
- ‘단일 책임 원칙’을 지킬 수 있도록 만들어 줌
- 스프링 프레임워크의 경우 트랜잭션 처리를 위해 데코레이터 패턴을 사용

### 3) 데코레이터 패턴을 적용할 때 고려할 점

> 데코레이터 대상이 되는 타입의 기능 개수
> 
- 정의되어 있는 메서드가 증가하게 되면 그 만큼 데코레이터의 구현도 복잡

> 데코레이터 객체가 비정상적으로 동작할 때 처리 방식
> 
- 스프링 프레임워크의 경우 트랜잭션 처리를 위해 데코레이터 패턴을 사용
- 이 때, 커밋 이후 문제가 발생했다면 이를 익셉션 처리를해야하는가?
- 클라이언트의 요구는 이미 수행되었기 때문..

### 4) 데코레이터 패턴의 단점

- 사용자 입장에서 데코레이터 객체와 실제 구현 객체의 구분이 되지 않음
- 따라서, 코드만으로 기능이 어떻게 동작하는지 이해하기 어려움

## 6. 프록시(Proxy) 패턴

- 프록시 패턴은 실제 객체를 대신하는 프록시 객체를 사용해서 실제 객체의 생성이나 접근 등을 제어할 수 있도록 해주는 패턴

### 1) 프록시 객체를 사용할 수 있는 경우

- 어떤 작업이 메모리 낭비와 수행 시간이 길어서 프록시 객체가 해당 작업을 처리해야할 필요가 있는 경우
- 다른 객체에 접근하기 위해 본인 객체는 은닉하고 프록시 객체를 두고자 하는 경우

> 프록시 패턴 구조
> 

![Proxy Pattern 1](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/ProxyPattern1.png)

### 2) 프록시 패턴 적용하기

> Case
> 
- 쇼핑몰 UI를 구현
- 상품 리스트를 출력해야하는데 모든 상품 리스트 이미지를 가져오는데 로딩 시간이 오래걸림
- 현재 스크롤 화면에서 보이는 상품만 데이터를 가져오고 다음 상품은 스크롤을 내렸을 때, 보여주고 싶은 상황

> 설계 구조
> 

![Proxy Pattern 2](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/ProxyPattern2.png)

- Image 인터페이스는 이미지를 표현하는 기능에 대한 명세
- ListUI는 Image 타입을 이용해 화면에 이미지를 출력
- RealImage 클래스는 실제로 이미지 데이터를 로딩해서 메모리에 보관하는 콘크리트 클래스
- ProxyImage 클래스는 필요 시점까지 이미지를 로딩하지 않고 있다가, 필요하면 RealImage에 해당 기능을 위임

> 프록시 클래스
> 

```java
public class ProxyImage implements Image {
    private String path;
    private RealImage image;

    public ProxyImage(String path) {
        this.path = path;
    }

    public void draw() {
        if (image == null) {
            iamge = new RealImage(path) // 최초 접근 시 객체 생성
        }
        image.draw(); // RealImage 객체에 위임
    }
}
```

- 프록시 클래스는 필요 시점까지 실제 객체를 생성하지 않음
- 필요한 시점이 되었을 때, 실제 객체를 생성하고 생성된 객체에 책임을 위임
- 프록시 클래스를 사용하는 클라이언트 코드는 추상화된 인터페이스를 사용하기 때문에 타입이 실제 클래스인지 프록시 클래스인지 여부는 모름
- 또한 필요 시점에 대한 정책이 변경되더라도 클라이언트 코드는 영향을 받지 않음

### 3) 프록시 패턴 종류

> 가상 프록시 (Virtual Proxy)
> 
- 앞의 예제의 경우 가상 프록시
- 필요 시점에 실제 객체를 생성

> 보호 프록시 (Protection Proxy)
> 
- 실제 객체에 대한 접근을 제어하는 프록시
- 접근 권한이 있는 경우에만 실제 객체의 메서드를 실행하는 방식으로 구현

> 원격 프록시 (Remote Proxy)
> 
- 다른 프로세스에 존재하는 객체에 접근할 때 사용되는 프록시

### 4) 프록시 패턴을 적용할 때 고려할 점

> 객체를 누가 생성할 것인가?
> 
- 가상 프록시의 경우 프록시 클래스에서 실제 클래스를 생성
- 보호 프록시의 경우 프록시 객체를 생성할 때 실제 객체를 전달받음

> 구현 방식 (상속 vs 위임)
> 
- 앞의 예제는 위임을 통해 구현
- 상속 방식의 경우 위임 방식에 비해 구조가 단순
- 상속 방식의 경우 객체를 생성하는 순간 실제 객체가 생성되기 때문에 가상 프록시를 구현하는데 적합하지 않음
