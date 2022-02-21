# Builder Pattern

- Builder Pattern은 객체를 만들어서 반환하는 패턴이나 팩토리 메서드 패턴이나 추상 팩토리 패턴과는 다름
- Constructor에 들어갈 parameter의 수에 상관없이 차례로 받아들여 모든 parameter를 받은 뒤에 통합
<br/>

### Case : 생성자를 통해서만 멤버 변수를 받는다

- 어떤 멤버 변수의 값이 없을 경우
- 입력 데이터의 순서가 다를 경우

```java
/**
	제약 사항 : Person 객체는 한번 생성되면 readOnly여야 한다. 
**/

public class Person {
	private String name;
	private Integer age;
	private String email;

	public Person(String name, Integer age, String email){
		this.name = name;
		this.age = age;
		this.email = email
	}

	public String getName() {
		return name;
	}

	public Integer getAge() {
		return age;
	}

	public String getEmail() {
		return email;
	}
```

- 해당 클래스는 제약 사항으로 인해 setter()를 사용할 수 없음

```java
new Person("KMS", 20, null)
```

- 위의 상황처럼 생성 시에 모든 멤버 변수의 값을 초기화할 수 없는 경우나 인자의 순서를 다르게 전달할 경우가 발생할 수 있기 때문에 파라미터에 맞는 생성자를 작성해야 함

```java
public Person(String name, Integer age){
	this(name, age)
}
```
<br/>

## Solution by Builder Pattern

- [x]  불필요한 생성자를 만들지 않음
- [x]  생성자의 인자 순서와 상관없이 객체를 만들어 냄
- [x]  사용자 입장에서 직관적이여야 함

```java
/**
	Builder Pattern을 적용한 Builder Class
**/
public class PersonBuilder{
	private String name;
	private Integer age;
	private String email;

	public PersonBuilder setName(String name){
		this.name = name;
		return this;
	}

	public PersonBuilder setAge(String age){
		this.age = age;
		return this;
	}

	public PersonBuilder setEmail(String email){
		this.email = email;
		return this;
	}

	public Person build(){
		Person person = new Person(name, age, email);
		return person;
	}
}
```

```java
/**
	Use builder class to create origin class
**/
public class BuilderPattern {
	public static void main(String[] args){
		// 빌더 객체 생성
		PersonBuilder personBuilder = new PersonBuilder();

		/** 
			생성하고 싶은 원본 객체의 정보를 빌더 클래스의 setter를 사용
			메서드의 반환 타입이 빌더 클래스 자체이기 때문에 체이닝 방식을 적용할 수 있으며,
      원본 생성자의 순서에 상관없이 입력 가능.
      동일한 멤버 변수의 값을 설정하게 되면 후위에 입력된 값으로 설정
			최종적으로 build() 메서드를 호출함으로써 원본 클래스를 생성
		**/

		Person minseok = personBuilder
		    .setName("kms")
			.setAge(29)
			.setEmail("dheldh77@icloud.com")
			.setName("Minseok")
			.build();
	}
}
```

- 빌더 클래스를 사용하면 명시적이고 직관적이게 원본 인스턴스를 생성할 수 있음
<br/>

- Tip
    - 빌더 클래스를 꼭 원본 클래스와 분리할 필요는 없음.
    - 원본 클래스 내부에 빌더 클래스를 포함할 수 있음
    
    ```java
    // Person class 내부
    public class Person {
    	// 생략
    
    	public class PersonBuilder{
    		// 생략
    	}
    
    	public static PersonBuilder Builder(){
    		return new PersonBuilder();
    	}
    }
    
    // main
    Person minseok = Person.Builder().setName("minseok").setAge(29).build();
    ```
