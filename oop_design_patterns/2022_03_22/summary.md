## 10. 파사드 (Facade) 패턴

- 파사드 패턴은 어떤 소프트웨어의 다른 커다란 코드 부분에 대한 간략화된 인터페이스를 제공하는 객체
- 파사드 패턴은 서브 시스템을 감춰 주는 상위 수준의 인터페이스를 제공함으로써 ‘코드 중복'과 ‘직접적인 의존' 문제를 해결하는데 도움을 줌

> 파사드 패턴 구조
> 

![스크린샷 2022-03-21 오전 11.07.31.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5199c2e9-952b-4da3-9e4b-fc1cb1f06f6e/스크린샷_2022-03-21_오전_11.07.31.png)

### 1) 파사드 패턴 적용하기

> 파사드 패턴을 적용할 수 있는 경우
> 

![스크린샷 2022-03-21 오전 11.05.35.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6be77961-f5a1-42ab-b908-56a528cc5361/스크린샷_2022-03-21_오전_11.05.35.png)

- 위와 같은 설계 구조의 코드가 있다고 생각하자
- 위 설계 구조에서는 세 개의 클래스에서 동일한 Dao에 접근하면서 ‘코드 중복’과 ‘직접적인 의존'이라는 문제점을 가지고 있다.

> 파사드 패턴 적용하기
> 

![스크린샷 2022-03-21 오전 11.17.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e61422ac-8d7e-4e8d-82de-ddaedfde52c1/스크린샷_2022-03-21_오전_11.17.09.png)

- 파사드를 이용해서 ‘코드 중복'과 ‘직접적인 의존’을 해결
- 위 설계에서 ‘EmpReportDaoFacade(파사드)는 서브 시스템에 속한 각 Dao를 이용해 클라이언트가 원하는 데이터를 제공하기 위한 인터페이스를 제공
- 각 클라이언트는 파사드를 이용해 서브시스템에 간접적으로 접근
- 파사드 패턴을 적용하면 코드가 간결해지고 서브 시스템의 일부가 변경되더라도 그 여파는 파사드로 한정될 가능성이 높아짐

### 2) 파사드 패턴의 장점

- 중복된 코드가 제거되고 코드가 간결
- 직접적인 의존 제거
- 서브 시스템의 일부가 변경되더라도 그 여파는 파사드로 한정될 가능성이 높아짐
- 파사드를 인터페이스로 정의함으로써 클라이언트의 변경 없이 서브 시스템 자체를 변경

### 3) 파사드 패턴의 특징

- 파사드 패턴을 적용한다고 해서 서브 시스템에 대한 직접적인 접근을 완전히 막는 것은 아님
- 파사드 패턴은 단지 여러 클라이언트에 ‘중복된 서브 시스템 사용을 파사드로 추상화’
- 따라서, 공통된 기능은 파사드를 통해 접근할 수 있도록 하고, 그렇지 않은 경우 서브 시스템에 직접 접근하는 방식을 선택할 수도 있음

## 11. 추상 팩토리 (Abstract Factory) 패턴

- 추상 팩토리 패턴은 상세화된 서브 클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 군을 생성하기 위한 인터페이스를 제공
- 추상 팩토리 패턴은 팩토리 메서드 패턴을 좀 더 캡슐화한 방식

### 1) 추상 팩터리 패턴이 사용되는 경우

- 객체가 생성되거나 구성, 표현되는 방식과 무관하게 시스템을 독립적으로 만들 경우
- 여러 제품군 중 하나를 선택해서 시스템을 설정해야 하고 한 번 구성한 제품을 다른 것으로 대체할 수 있을 경우
- 관련된 제품 객체들이 함께 사용되도록 설계되었고, 이 부분에 대한 제약이 외부에도 지켜지도록 하고 싶은 경우
- 제품에 대한 클래스 라이브러리를 제공하고, 그들의 구현이 아닌 인터페이스를 노출 시키고 싶은 경우

### 2) 추상 팩토리 적용하기

> Shooting Game’s enemy
> 

![스크린샷 2022-03-21 오후 2.20.00.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3bd41c61-9235-45ec-98bd-4e86e56229fd/스크린샷_2022-03-21_오후_2.20.00.png)

```bash

public class Stage {
    private void createEnemies() {
        for (int i = 0 ; i <= ENEMY_COUNT ; i++) {
            if (stageLevel == 1) {
                enemies[i] = new DashSmallFlight(1, 1); // 공격력 / 수비력 
            } else if (stageLevel == 2) {
                enemies[i] = new MissileSmallFlight(1, 1);
            }
        }
        
        if (stageLevel == 1) {
            bose = new StrongAttackBoss(1, 10);
        } else if (stageLevel == 2) {
            boss = new CloningBoss(5, 20);
        }
    }
    
    private void createObstacle() {
        for (int i = 0 ; i < OBSTACLE_COUNT ; i++) {
            if (stageLevel == 1) {
                obstacle[i] = new RockObstacle();
            } else if (stageLevel == 2) {
                obstacle[i] = new BombObstacle();
            }
        }
    }
}
```

- 슈팅 게임을 예시로 들어보자
- 스테이지에 따라 Boss, SmallFlight, Obstacle이 존재한다.
- Stage 클래스는 조건문을 사용해 StageLevel에 따라 Stage에 맞는 인스턴스를 생성하고 있다.
- 위의 코드처럼 작성하게 되면 새로운 클래스가 추가 또는 변경될 때마다 객체를 사용하는 코드를 변경해야한다.
- 또한, 중첩된 if-else문 비슷한 코드블럭은 코드를 복잡하게 만들고 유지보수를 어렵게 만든다.

> 추상 팩토리 패턴 적용하기
> 
- 위 예시에서 클라이언트 클래스로부터 객체 생성 책임을 분리하는 것이 추상 팩토리 패턴
- 추상 팩토리(Abstract Factory) 패턴에서는 관련된 객체 군을 생성하는 책임을 갖는 타입을 별도로 분리

![스크린샷 2022-03-21 오후 2.49.27.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0706091e-cd79-4f83-89f8-c5cd66b97c18/스크린샷_2022-03-21_오후_2.49.27.png)

- 추상 팩토리 클래스는 객체 생성 메서드를 선언하는 추상 타입으로 팩토리에 해당
- 추상 팩토리 클래스의 getFactory() 메서드는 정적 메서드로써 파라미터로 전달받은 레벨에 따라 알맞은 객체를 리턴하도록 정의

> 추상 팩토리 패턴의 추상 팩토리 구현
> 

```java
public abstract class EnemyFactory {
    public static EnemyFactory getFactory(int level) {
        if (level == 1)
            return EasyStageEnemyFactory();
        else
            return HardStageEnemyFactory();
    }

    // 객체 생성을 위한 팩토리 메서드
    public abstract Boss createBoss();
    public abstract SmallFlight createSmallFlight();
    public abstract Obstacle createObstacle();
}
```

> 콘크리트 팩토리 클래스 구현
> 

```java
public class EasyStageEnemyFactory extends EnemyFactory{
    public Boss createBoss() {
        return new StrongAttackBoss();
    }
    public SmallFlight createSmallFlight() {
        return new DashSmallFlight();
    }

    public Obstacle createObstacle() {
        return new RockObstacle();
    }
}

public class HardStageEnemyFactory extends  EnemyFactory {
    public Boss createBoss() {
        return new CloningAttackBoss();
    }

    public SmallFlight createSmallFlight() {
        return new MisilleSmalFlight();
    }

    public Obstacle createObstacle() {
        return new BombObstacle();
    }
}
```

> 팩토리를 사용하는 클라이언트 클래스
> 

```java
public class Stage {
    private EnemyFactory enemyFactory;
    private Boss boss;
    private SmallFligth enemies[];
    private Obstacle obstacles[];

    public Stage (int level) {
        enemyFactory = EnemyFactory.getFactory(level);
    }

    private void createEnemy() {
        for (int i = 0 ; i <= ENEMY_COUNT ; i++ ) {
            enemies[i] = enemyFactory.createSmallFlight();
        }

        boss = enemyFactory.createBoss();
    }

    private void createObstacle() {
        for (int i = 0 ; i <= OBSTACLE_COUNT ; i++ ) {
            obstacles[i] = enemyFactory.createObstacle();
        }
    }
}
```

- 변경된 클라이언 클래스를 보면 콘크리트 제품 클래스가 아닌 추상 타입만을 사용
- 각 콘크리트 제품 클래스를 사용해서 객체를 생성하는 코드는 팩토리의 하위 타입에서 구현
- 클라이언트 클래스는 팩토리 클래스의 정적 메서드를 통해 사용할 팩토리 클래스를 반환받는데, 새로운 타입으로 변경하고 싶다면, 정적 메서드를 변경할 필요 없이 팩토리 구현 클래스를 만들고 정적 메서드에 해당 객체를 리턴해주도록 수정해주면 됨

> DI를 사용한 팩토리 패턴
> 

```java
public class Stage {
    private int level;
    private EnemyFactory enemyFactory;

    // DI를 적용하면 팩토리를 구하는 기능을 EnemyFactory에 구현할 필요가 없음
    public Stage(int level, EnemyFactory enemyFactory) {
        this.level = level;
        this.enemyFactory = enemyFactory;
    }
}
```

- DI를 사용하면 생성자나 setter를 통해 EnemyFactory 객체를 전달받게 되므로, EnemyFactory의 정적 메서드를 정의할 필요가 없어짐
- 따라서 EnemyFactory 추상 클래스를 인터페이스로 사용할 수 있게 됨

### 3) 추상 팩토리 패턴의 장점

- 클라이언트에 영향을 주지 않으면서 사용할 제품(객체)를 교체할 수 있음
- 만약 팩토리가 생성하는 객체가 늘 동일한 상태를 갖는다면, ‘프로토타입 방식'으로 팩토리 패턴을 구현할 수 있음

> 프로토타입 방식의 팩토리 패턴
> 

```java
public class Factory {
    private ProductA productAProto;
    private ProductB productBProto;
    
    public Factory (ProductA productAProto, ProductB productBProto) {
        this.productAProto = productAProto;
        this.productBProto = productBProto;
    }
    
    public ProductA createA () {
        return (ProductA) productAProto.clone();
    }
    
    public ProductB createB () {
        return (ProductB) productBProto.clone();
    }
}
```

- 프로토타입 방식은 생성할 객체의 원형 객체를 등록하고, 객체 생성 요청이 있으면 원형 객체를 ‘복제'해서 생성
- 객체 군 마다 팩토리 클래스를 작성할 필요 없이 팩토리 객체를 생성
- 프로토타입 방식을 사용하면 추상 팩토리 타입과 콘크리트 팩토리 클래스를 따로 만들 필요가 없어 구현이 쉽지만, 제품 객체의 생성 규칙이 복잡할 경우 적용할 수 없는 한계
