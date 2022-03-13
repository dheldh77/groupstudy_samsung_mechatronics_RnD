# Template Method Pattern

- [Template Method Pattern Reference](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/summary.md#3-%ED%85%9C%ED%94%8C%EB%A6%BF-%EB%A9%94%EC%84%9C%EB%93%9Ctemplate-method-%ED%8C%A8%ED%84%B4)

# Strategy Pattern

- [Strategy Pattern Reference](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/summary.md#1-%EC%A0%84%EB%9E%B5strategy-%ED%8C%A8%ED%84%B4)

# ‘Template Pattern’ 과 ‘Strategy Pattern’ 차이

- 특정 로직에 대해 ‘템플릿 메서드 패턴'을 사용할 수도 있고, ‘전략 패턴'을 사용할 수도 있음
- 다형성을 활용하는 방법이 다를 뿐, 목적은 동일하게 이룰 수 있음

# 사용 시기

## 1. 전략 패턴(Strategy Pattern)

### 1) 런타임 때 로직을 변경해야 하는 경우

- 전략 패턴은 컨텍스트 클래스가 전략을 속성(필드, 인스턴스 변수)으로 가지고 있다보니, setter를 통해 런타임 도중 전략을 변경 가능
- 템플릿 메서드 패턴의 경우 로직을 변경하려면 클래스로부터 인스턴스를 새롭게 생성해야 함. 따라서 런타임 도중 로직을 변경할 수 없음

### 2) 컨텍스트 클래스가 상속을 받은 클래스인 경우

- 템플릿 메서드 패턴은 기본적으로 상속을 활용한 구조
- 따라서, 컨텍스트 클래스가 상속을 받고 있다면, 템플릿 메서드 패턴을 사용하는 순간 상속의 깊이가 깊어지고 유지보수가 힘들어짐
- 전략 패턴의 경우 ‘상속 구조'를 사용하지 않기 때문에 상속 깊이가 깊어지지 않음

### 3) 다양한 컨텍스트 클래스에서 전략을 공유하고 싶은 경우

- 템플릿 메서드 패턴은 로직의 순서가 고정되어 있어, 고정된 로직 순서와 일치하는 컨텍스트 클래스에서 밖에 사용할 수 없음
- 전략 패턴은 전략이 클래스로 분리되어 있어, 로직의 순서와 상관없이 전략에서 필요한 메서드를 배치해서 사용 가능

## 2. 템플릿 메서드 패턴(Template Method Pattern)

### 1) 위에서 언급한 경우를 제외한 경우

- 템플릿 메서드 패턴은 상속과 추상 클래스를 사용해 구현
- 따라서, 직관적으로 코드를 구현할 수 있고, 의존 주입에 대한 코드를 작성하지 않아도 되어서 간단하게 구현 가능
