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
