# Chapter 06. DI(Dependecy Injection)와 서비스 로케이터

- 로버트 C 마틴(Robert C. Martin)은 소프트웨어를 어플리케이션 영역과 메인 영역으로 구분해서 설명
- 어플리케이션 영역은 고수준 정책 및 저수준 구현을 포함하는 영역
- 어플리케이션이 동작하도록 각 객체들을 연결해 주는 영역

## 1. 어플리케이션 영역과 메인 영역

```java
public class Main {
    public static void main(String[] args) {
        // 상위 수준 모듈인 transcoder 패키지에서 사용할 하위 수준 모듈 객체 생성
        JobQueue jobQueue = new FileJobQueue()
        Transcoder transcoder = new FfmpegTranscoder();

        // 상위 수준 모듈이 하위 수준 모듈을 사용할 수 있도록 Locator 초기화
        Locator locator = new Locator(jobQueue, transcoder);
        Locator.init(locator);

        // 상위 수준 모듈 객체를 생성하고 실행
        final Worker worker = new Worker();
        Thread t = new Thread(new Runnable() {
            public void run() {
                worker.run();
            }
        });
    }
}
```

- 메인 영역은 어플리케이션 영역의 객체를 생성, 설정, 실행하는 책임
- 어플리케이션 영역에서 사용할 하위 수준의 모듈을 변경하고 싶다면 메인 영역을 수정
- 모든 의존은 메인 영역에서 어플리케이션 영역으로 향함
- 따라서, 메인 영역을 변경한다고 해도 어플리케이션 영역은 변경되지 않음
- 위 코드에서 보듯이 사용할 객체를 제공하는 책임을 갖는 객체를 서비스 로케이터(Serivce Locator)라고 부름
- 서비스 로케이터 방식은 로케이터를 통해 필요한 객체를 직접 찾는 방식
- 서비스 로케이터에는 단점이 존재하기 때문에 외부에서 사용할 객체를 주입해주는 DI(Dependency Injection) 방식을 사용하는 것이 일반적

## 2. DI(Dependency Injection)을 이용한 의존 객체 사용

- 콘크리트 클래스를 직접 사용해 객체를 생성하게 되면 의존 역전 원칙을 위반하고 결과적으로 확장 폐쇄 원칙을 위반
- 서비스 로케이터를 통해 의존 객체를 찾게 되면 몇가지 단점이 발생
- 서비스 로케이터의 단점을 보완하는 방법이 DI(Dependency Injection)으로 필요한 객체를 직접 생성하거나 찾지 않고 외부에서 넣어 주는 방식

```java
public class Worker {
    private JobQueue jobQueue;
    private Transcoder transcoder;

    // 외부에서 사용할 객체를 전달받을 수 있는 방법 제공
    // 생성자를 이용한 의존 객체 주입
    public Worker(JobQueue jobQueue, Transcoder transcoder) {
        this.jobQueue = jobQueue;
        this.transcoder = transcoder;
    }

    public void run() {
        while(someRunningCondition){
            JobData jobData = jobQueue.getJob();
            transcoder.trnascode(jobData.getSource(), jobData.getTarget());
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // 상위 수준 모듈인 transcoder 패키지에서 사용할 하위 수준 모듈 객체 생성
        JobQueue jobQueue = new FileJobQueue()
        Transcoder transcoder = new FfmpegTranscoder();

        // 상위 수준 모듈 객체를 생성하고 실행
        final Worker worker = new Worker(jobQueue, transcoder);
        Thread t = new Thread(new Runnable() {
            public void run() {
                worker.run();
            }
        });
        JobCLI cli = new JobCLI();
        cli.interact();
    }
}
```

- 위 코드 예시에서 보듯이 생성자를 통해 외부에서 의존하는 객체를 넣어 주기 때문에, 이런 방식을 의존(Dependency) 주입(Injection) 방식이라고 부름
- DI를 통해 의존 객체를 관리할 때에는 객체를 생성하고 각 객체들을 의존 관계에 따라 연결해주는 조립기가 필요
- 조립기를 별도로 분리하면 향후에 조립기 구현 변경의 유연함을 얻을 수 있음

```java
public class Worker {
    private JobQueue jobQueue;
    private Transcoder transcoder;

    // 외부에서 사용할 객체를 전달받을 수 있는 방법 제공
    // 생성자를 이용한 의존 객체 주입
    public Worker(JobQueue jobQueue, Transcoder transcoder) {
        this.jobQueue = jobQueue;
        this.transcoder = transcoder;
    }

    public void run() {
        while(someRunningCondition){
            JobData jobData = jobQueue.getJob();
            transcoder.trnascode(jobData.getSource(), jobData.getTarget());
        }
    }
}

public class Assembler {
    public void createAndWire() {
        JobQueue jobQueue = new FileJobQueue()
        Transcoder transcoder = new FfmpegTranscoder();
        this.worker = new Worker(jobQueue, transcoder);
    }
    public Worker getWorker() {
        return this.worker
    }
}

public class Main {
    public static void main(String[] args) {
        Assembler assembler = new Assembler();
        assembler.createAndWire();
        final Worker worker = assembler.getWorker();
    }
}
```

- 객체 조립 기능이 분리되면 이후에 XML 파일을 이용해 객체 생성과 조립에 대한 정보를 설정하고, XML 파일을 읽어 와 초기화 해주도록 구현을 변경
- 스프링 프레임워크가 객체를 생성하고 조립해 주는 기능을 제공하는 DI 프레임워크

### 1) 생성자 방식과 설정 메서드 방식

- DI를 적용해 의존 객체를 전달하는 방법은 크게 ‘생성자 방식'과 ‘설정 메서드 방식'이 존재

(1) 생성자 방식

- 생성자를 통해 의존 객체를 전달
- 생성자를 통해 전달받은 객체를 필드에 보관할 뒤, 메서드에서 사용

(2) 설정 메서드 방식

- setter를 통해 의존객체를 필드에 보관
- 다른 메서드에서 필드를 사용해 의존 객체를 실행

(3) 각 방식의 장단점

- ‘생성자 방식’은 객체를 생성하는 시점에 필요한 모든 의존 객체를 준비
- 생성 시점에 의존 객체를 모두 받기 때문에, 한 번 객체가 생성되면 객체가 정상적으로 동작함을 보장(👍)
- 의존 객체를 먼저 생성할 수 없다면 생성자 방식을 사용할 수 없음(👎)
<br/>

- ‘설정 메서드 방식'은 객체를 생성한 뒤에 의존 객체를 주입
- 의존 객체를 설정하지 못한 상태에서 객체를 사용하게 되면 NullPointerException이 발생(👎)
- 어떠한 이유로 의존 객체가 나중에 생성될 경우에 사용 가능(👍)
- 의존할 객체가 많을 경우, 메서드 이름을 통해 의존을 주입하므로 가독성을 높여줌(👍)

### 2) DI와 테스트

- 단위 테스트는 한 클래스의 기능을 테스트하는데 초점
- 테스트할 클래스가 DI 패턴을 따른다면, 생성자나 설정 메서드를 이용해 Mock 객체를 쉽게 전달할 수 있음
- 의존하는 객체의 구현이 완료되었는지 여부와 상관없이 Mock  객체를 이용해 테스트할 수 있고, Mock 객체를 생성하기 위해 기존의 다른 코드를 변경할 필요가 없음

### 3) 스프링 프레임워크 예

- DI 프레임 워크인 스프링 프레임워크는 생성자 방식과 설정 메서드 방식을 모두 지원
- 스프링은 XML 파일을 이용해 객체를 어떻게 생성하고 조립하지 설정
- 스프링 XML 설정에서는 순서나 이름이 잘 드러나지 않는 생성자 방식 보다는 어떤 프로퍼티에 의존을 설정하는지 잘드러나는 설정 메서드 방식이 가독성을 높여 준다.
- 스프링은 객체를 생성하고 연결해 주는 Assembler의 역할을 수행

> 생성자 방식
> 

```xml
<bean id="worker" class="transcoder.Worker">
    <constructor-arg ref="fileJobQueue" />
    <constructor-arg ref="ffmpegTranscoder" />
</bean>
```

- <constructor-arg> 태그는 생성자에 전달할 객체를 지정할 때 사용

> 설정 메서드 방식
> 

```xml
<bean id="worker" class="transcoder.Worker">
    <property name="jobQueue" ref="fileJobQueue" />
    <property name="transcoder" ref="ffmegTranscoder" />
</bean>
```

- <property> 태그는 설정 메서드 방식을 이용

> XML 설정 방식의 장/단점
> 
- 의존할 객체가 변경될때 자바 코드를 수정하고 컴파일할 필요 없이 XML 파일만 수정(👍)
- 스프링은 XML 기반의 다양한 설정 방법을 제공하고 있어 유연하게 객체 조립을 설정(👍)
- 개발자가 입력한 오타에 다소 취약(👎)

> 자바 기반 설정 방식의 장/단점
> 
- 스프링 3 버전부터는 자바 코드 기반의 설정 방식이 추가
- 오타로 인한 문제가 거의 발생하지 않는다(👍)
- 의존 객체를 변경해야 할 경우, 자바 코드를 수정해서 다시 컴파일하고 배포(👎)
    
## 3. 서비스 로케이터를 이용한 의존 객체 사용

- 개발 환경이나 사용하는 프레임워크의 제약으로 DI 패턴을 적용할 수 없는 경우가 있음
- 안드로이드 플랫폼의 경우 화면을 생성할 때 Activity 클래스를 상속받로고 하고 있는데, 안드로이드 실행 환경은 정해진 메서드만을 호출할 뿐, 안드로이드 프레임워크가 DI 처리를 위한 방법을 제공하지는 않음
- 이 경우에 서비스 로케이터를 이용할 수 있음

### 1) 서비스 로케이터의 구현

- 서비스 로케이터는 어플리케이션에서 필요로 하는 객체를 제공하는 책임
- 의존 대상이 되는 객체 별로 제공 메서드를 정의
- 의존 객체가 필요한 코드에서는 서비스 로케이터가 제공하는 메서드를 이용해 필요한 객체를 구함
- 서비스 로케이터가 올바르게 동작하려면 서비스 로케이터 스스로 어떤 객체를 제공해야할지 알아야 함
- DI와 마찬가지로 메인영역에서 서비스 로케이터가 제공할 객체를 초기화
- 서비스 로케이터는 어플리케이션 영역의 객체에서 직접 접근하기 때문에 어플리케이션 영역에 위치
- 메인 영역에서는 서비스 로케이터가 제공할 객체를 생성 후 서비스 로케이터를 초기화
- 서비스 로케이터를 구현하는 방법은 ‘객체 등록' 방식과 ‘상속' 방식 등이 있음

> 객체 등록 방식의 서비스 로케이터 구현
> 
- 서비스 로케이터를 생성할 때 사용할 객체를 전달
- 서비스 로케이터 인스턴스를 지정하고 참조하기 위한 static 메서드를 제공

```java
import java.security.Provider.Service;

public class ServiceLocator {
    private JobQueue JobQueue;
    private Transcoder transcoder;

    // constructor를 이용해 의존 객체 전달
    public ServiceLocator(JobQueue JobQueue, Transcoder transcoder) {
        this.JobQueue = JobQueue;
        this.transcoder = transcoder;
    }

    // 서비스 로케이터가 제공할 객체가 많을 경우, 각 객체마다 별도의 setter를 만들어 가독성을 높일 수 있음
    public void setJobQueue(JobQueue jobQueue) {
        this.JobQueue = jobQueue;
    }

    public void setTranscoder(Transcoder transcoder) {
        this.transcoder = transcoder;
    }

    // getter
    public JobQeuue getJobQueue() {
        return jobQueue;
    }

    // getter
    public Transcoder getTranscoder() {
        return transcoder;
    }

    // service locator 접근 위한 static method
    private static SerivceLocator instance;
    
    public static void load(SerivceLocator locator) {
        ServiceLocator.instance = locator;
    }

    public static ServiceLocator getInstance() {
        return instance;
    }
}

public static void main(String[] args){
    // 의존 객체 생성
    FileJobQueue jobQueue = new FileJobQueue();
    FfmpegTranscoder transcoder = new FfmpegTranscoder();

    // 서비스 로케이터 초기화
    // 메인 영역에서 SeriveLocator의 생성자를 이용해 서비스 로케이터가 제공할 객체를 설정
    SerivceLocator locator = new SerivceLocator(jobQueue, transcoder);
    // load() 메서드로 메인 영역에서 사용할 ServiceLocator 초기화
    SerivceLocator.load(locator);

    // 어플리케이션 실행 코드
    Worker worker = new Worker();
}

public class Worker {
    public void run() {
        // 서비스 로케이터를 이용해 의존 객체를 구함
        // 어플리케이션 영역에서는 서비스 로케이터가 제공하는 메서드를 이용해 필요한 객체를 구한뒤 해당 객체의 기능 수행
        ServiceLocator locator = ServiceLocator.getInstance();
        JobQueue jobQueue = locator.getJobQueue();
        Transcoder transcoder = locator.getTranscoder();

        // 의존 객체 사용
        while(someRunningCondition) {
            JobData jobData = jobQueue.getJob();
            transcoder.transcode(jobData.getSource(), jobData.getTarget());
        }
    }
}
```

- 객체를 등록하는 방식은 구현이 쉬움
- 생성자나 setter를 통해 의존 객체를 등록한 뒤, 어플리케이션 영역에서 사용만 하면 됨
- 객체를 등록하는 인터페이스가 노출되어 있기 때문에 어플리케이션 영역에서 얼마든지 의존 객체를 바꿀 수 있음
- 이는, 고수준 모듈에서 콘크리트 클래스에 직접 접근하도록 유도할 수 있기 때문에, 의존 역전 원칙을 어기게 만드는 원인

```java
public class Worker {
    public void run() {
        // 고수준 모듈에서 저수준 모듈에 직접 접근 유도
        SerivceLocator oldLocator = SerivceLocator.getInstance();
        SerivceLocator newLocator = new SerivceLocator(
            // 의존 역전 원칙 위반
            new DbJobQueue(), oldLocator.getTranscoder() );
        )
        SerivceLocator.load(newLocator);

        ...
    }
}
```

> 상속을 통한 서비스 로케이터 구현
> 
- 객체를 구하는 추상 메서드를 제공하는 상위 타입 구현
- 상위 타입을 상속받은 하위 타입에서 사용할 객체 설정

```java
// 추상 클래스
// 이 클래스를 상속받아 추상 메서드의 구현을 제공하는 클래스가 필요
// 상속 받은 클래스가 어플리케이션에서 실제로 필요로 하는 의존 객체를 생성
public abstract class SerivceLocator {
    public abstract JobQueue getJobQueue();
    public abstract Transcoder getTranscoder();

    // protected 공개 범위의 생성자는 static 필드인 instance에 자기 자신을 할당
    protected ServiceLocator() {
        SerivceLocator.instance = this;
    }

    private static ServiceLocator instance;
    public static ServiceLocator getInstance() {
        return instance;
    }
}

public class MySerivceLocator extends SerivceLocator {
    private FileJobQueue jobQueue;
    private FfmpegTranscoder transcoder;

    public MySerivceLocator() {
        super();
        this.jobQueue = new FileJobQueue();
        this.transcoder = new FfmpegTranscoder();
    }

    @Override
    public JobQueue getJobQueue() {
        return jobQueue;
    }

    @Override
    public Transcoder getTranscoder() {
        return transcoder;
    }
}
```

- ServiceLocator 클래스를 상속받은 클래스는 메인 영역에 위치
- 의존 객체를 교체해야할 때, 어플리케이션 영역의 코드 수정 없이 메인 영역의 코드만 수정

> 제네릭/템플릿을 이용한 서비스 로케이터 구현
> 
- 서비스 로케이터의 단점은 인터페이스 분리 원칙을 위반한다는 것
- 서비스 로케이터를 사용함으로써 사용하지 않는 타입에 대한 의존이 함께 발생
- 서비스 로케이터를 사용하는 코드가 많아질수록 문제가 배로 발생
- 이를 해결하기 위해 의존 객체마다 서비스 로케이터를 작성해 인터페이스를 분리
- 제네릭/템플릿을 이용하면 중복 코드 없이 서비스 로케이터로 인터페이스를 분리한 효과를 얻을 수 있음

```java
public class SerivceLocator {
    private static Map<Class<?>, Object> objectMap =
        new HashMap<Class<?>, Object>();

    public static <T> T get(Class<T> klass) {
        return (T) objectMap.get(klass);
    }

    public static void regist(Class<?> klass, Object obj) {
        objectMap.put(klass, obj);
    }
}
```

### 2) 서비스 로케이터의 단점

- 동일 타입의 객체가 다수 필요한 경우, 각 객체 별로 제공 메서드를 만들어 주어야 한다는 점
- 조건에 따라 다른 메서드를 호출하는 코드가 들어가게 될 경우 요구 사항이 추가되었을 때 기능 확장을 위해 코드를 수정하게 되고 개방 폐쇄 원칙을 위반
- 또한, 인터페이스 분리 원칙 위반
- 부득이한 상황이 아니라면 DI를 쓰도록!
