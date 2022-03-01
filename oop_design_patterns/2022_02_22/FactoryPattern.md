# 1. 생성 패턴(Creational Pattern)

- 팩토리 패턴은 생성 패턴 중 하나
- 생성 패턴이란 인스턴스를 만드는 절차를 추상화하는 패턴
- 생성 패턴에 속하는 패턴은 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리
- 생성 패턴은 시스템이 상속(Inheritance)보다 복합(Composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있음

> 생성 패턴의 중요 이슈
> 
- 생성 패턴은 시스템이 어떤 Concrete class를 사용하는지에 대한 정보를 캡슐화
- 생성 패턴은 클래스의 인스턴스들을 만들고 결합하는 방법에 대해 완전히 가려줌

# 2. 팩토리 메소드 패턴 (Factory Method Pattern)

- 팩토리 메소드 패턴은 객체를 생성하는 인터페이스를 미리 정의하되, 인스턴스를 만들 클래스의 결정은 서브 클랙스에게 위임
- 즉, 여러 개의 서브 클래스를 가진 슈퍼 클래스가 있을 때, 입력에 따라 하나의 자식 클래스의 인스턴스를 반환
- 팩토리 메소드 패턴은 인스턴스화에 대한 책임을 객체를 사용하는 클라이언트에서 팩토리 클래스로 위임

## 1) When?

- 어떤 클래스가 자신이 생성해야하는 객체의 클래스를 에측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브 클래스가 지정했으면 할 때

## 2) Example

> 슈퍼 클래스
> 

```java
public abstract class Employee {
    public abstract String getTask();
    public abstract String getMajor();
    public abstract String getDegree();

    @Override
    public String toString() {
        return "Task= " + this.getTask() + ", Major= " + this.getMajor() + ", Degree= " + this.getDegree();
    }
}
```

- 회사 직군과 관련된 Employee라는 슈퍼 클래스 생성

> 서브 클래스
> 

```java
public class Developer extends Employee {
    private String task;
    private String major;
    private String degree;

    public Developer(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public String getTask() {
        return this.task;
    }

    @Override
    public String getMajor() {
        return this.major;
    }

    @Override
    public String getDegree() {
        return this.degree;
    }
}

public class Analyst extends Employee {
    private String task;
    private String major;
    private String degree;

    public Analyst(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public String getTask() {
        return this.task;
    }

    @Override
    public String getMajor() {
        return this.major;
    }

    @Override
    public String getDegree() {
        return this.degree;
    }
}
```

- 슈퍼 클래스를 상속받은 서브 클래스는 업무에 따라 Developer와 Analyst로 나뉨

> 팩토리 클래스
> 

```java
public class EmployeeFactory {
    public static Employee getEmployee(String task, String major, String degree) {
        if("Analysis".equalsIgnoreCase(task))
            return new Analyst(task, major, degree);
        else if("Development".equalsIgnoreCase(task))
            return new Developer(task, major, degree);
        return null;
    }
}
```

- 팩토리 클래스에서는 해당 업무에 맞는 서브 클래스를 리턴
- 팩토리 클래스에서 인스턴스를 리턴하는 함수는 static으로 선언되어 있음
- 팩토리 클래스를 이용해 인스턴스를 필요로하는 Application에서 Employee의 서브  클래스에 대한 정보는 모른채 인스턴스를 선언할 수 있음
- 또한, Employee 클래스를 상속받는 서브 클래스가 추가된다고 해도 인스턴스를 제공받던 Application은 수정할 필요가 없음

> 클라이언트 코드
> 

```java
public class TestFactory {
    public static void main(String[] args) {
        Employee analyst = EmployeeFactory.getEmployee("Analysis", "mechanical engineering", "master");
        Employee developer = EmployeeFactory.getEmployee("Development", "electronics", "bachelor");

        System.out.println("Analyst info : " + analyst);
        System.out.println("Developer Info : " + developer);
    }
}
```

## 3) 팩토리 메소드 패턴 구현

- 팩토리 클래스를 싱글톤으로 구현해도 되고, 서브 클래스를 리턴하는 static 메소드로 구현해도 됨
- 팩토리 메소드는 입력된 파라미터에 따라 다른 서브 클래스의 인스턴스를 생성하고 리턴

## 4) 장점

- 클라이언트 코드로부터 서브 클래스의 인스턴스화를 제거하므로 종속성을 낮추고, 결합도를 느슨하게 하며(Loosely Coupled), 확장을 쉽게 함

# 3. 추상 팩토리 패턴(Abstract Factory Pattern)

- 팩토리 메소드 패턴의 경우 하나의 팩토리 클래스가 입력에 따라 if-else나 switch문을 사용해 다양한 서브클래스를 리턴하는 방식
- 추상 팩토리 패턴은 팩토리 클래스에서 서브 클래스를 생성하는데 있어 이러한 if-else문을 제거
- 추상 팩토리 패턴은 입력으로 서브 클래스에 대한 식별을 하는 것이 아닌 또 하나의 팩토리 클래스를 받음

## 1) Example

> 슈퍼 클래스
> 

```java
public abstract class Employee {
    public abstract String getTask();
    public abstract String getMajor();
    public abstract String getDegree();

    @Override
    public String toString() {
        return "Task= " + this.getTask() + ", Major= " + this.getMajor() + ", Degree= " + this.getDegree();
    }
}
```

- 회사 직군과 관련된 Employee라는 슈퍼 클래스 생성

> 서브 클래스
> 

```java
public class Developer extends Employee {
    private String task;
    private String major;
    private String degree;

    public Developer(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public String getTask() {
        return this.task;
    }

    @Override
    public String getMajor() {
        return this.major;
    }

    @Override
    public String getDegree() {
        return this.degree;
    }
}

public class Analyst extends Employee {
    private String task;
    private String major;
    private String degree;

    public Analyst(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public String getTask() {
        return this.task;
    }

    @Override
    public String getMajor() {
        return this.major;
    }

    @Override
    public String getDegree() {
        return this.degree;
    }
}
```

- 슈퍼 클래스를 상속받은 서브 클래스는 업무에 따라 Developer와 Analyst로 나뉨

> 추상 팩토리 (인터페이스 or 추상 클래스)
> 

```java
public interface EmployeeAbstractFactory {
    public Employee createEmployee();
}
```

- 팩토리 인터페이스의 생성 메서드의 리턴 타입이 서브 클래스가 아닌 슈퍼 클래스
- 팩토리 인터페이스를 구현하는 클래스에서 생성 메서드를 오버라이딩해 각각의 서브 클래스를 리턴
- 다형성을 활용한 방식

> 팩토리 구현 클래스
> 

```java
public class AnalystFactory implements EmployeeAbstractFactory {
    private String task;
    private String major;
    private String degree;

    public AnalystFactory(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public Employee createEmployee() {
        return new Analyst(task, major, degree);
    }
}

public class DeveloperFactory implements EmployeeAbstractFactory {
    private String task;
    private String major;
    private String degree;

    public DeveloperFactory(String task, String major, String degree) {
        this.task = task;
        this.major = major;
        this.degree = degree;
    }

    @Override
    public Employee createEmployee() {
        return new Developer(task, major, degree);
    }
}
```

- 각 팩토리 구현 클래스에서 슈퍼 클래스의 생성 메소드를 오버라이딩해 서브 클래스를 리턴

> 컨슈머(Consummer) 클래스
> 

```java
public class EmployeeFactory {
    public static Employee getEmployee(EmployeeAbstractFactory employee) {
        return employee.createEmployee();
    }
}
```

- 서브 클래스를 생성하기 위해 클라이언트 코드에 접점으로 제공되는 컨슈머 클래스
- 클라이언트 코드는 컨슈머 클래스의 static 메서드를 통해 팩토리 구현 클래스의 인스턴스를 넣어줌으로써 if-else 문 없이도 원하는 서브 클래스의 인스턴스를 생성

> 클라이언트 코드
> 

```java
public class AbstractFactoryTest {
    public static void main(String[] args){
        Employee analyst = EmployeeFactory.getEmployee(new AnalystFactory("Analysis", "mechanical engineering", "master"));
        Employee developer = EmployeeFactory.getEmployee(new AnalystFactory("Development", "electronics", "bachelor"));

        System.out.println("Analyst info : " + analyst);
        System.out.println("Developer Info : " + developer);
    }
}
```

## 2) 장점

- 추상 팩토리 패턴은 구현(Implemets)보다 인터페이스(Interface)를 위한 코드 접근법 제공
- 요구 사항의 변경으로 서브 클래스를 확장하는데 용이 (팩토리 구현 클래스만 추가하면 됨)
- if-else문 제거
