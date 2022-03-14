# 전략 패턴(Strategy Pattern)

- [Strategy Pattern](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/summary.md#1-%EC%A0%84%EB%9E%B5strategy-%ED%8C%A8%ED%84%B4)

# 상태 패턴(State Pattern)

- [State Pattern](https://github.com/dheldh77/groupstudy_samsung_mechatronics_RnD/blob/master/oop_design_patterns/2022_03_08/summary.md#4-%EC%83%81%ED%83%9Cstate-%ED%8C%A8%ED%84%B4)

# 전략 패턴과 상태 패턴의 차이점

## 1) 구분 기준

- 전략 패턴은 한 번 인스턴스를 생성하고 나면, 상태가 거의 바뀌지 않는 경우에 사용
- 상태 패턴은 한 번 인스턴스를 생성하고 난 뒤, 상태가 빈번하게 바뀌는 경우에 사용

## 2) 전략 패턴 예시

- 우리 회사의 각 직군에 대해 일반화해서 생각해보자.
- 직군은 e직군, s직군, f직군, m군 등으로 나뉜다.
- 직원이라는 인스턴스를 생성하게 되면, 각 직군으로 들어온 직원(인스턴스)은 퇴사해서 인스턴스가 사라지기 전까지(일반화해서 생각하므로 직군 전환, 능력자 등에 대한 것은 제외)  입사했을 때 가지는 업무(기능)이 정해지게 된다.

## 3) 상태 패턴 예시

- 우리 획사의 직급에 대해 일반화 해서 생각해보자
- CL1, CL2, CL3, CL4 등으로 직급이 나뉜다.
- 각 직급(상태)에 따라 월급 수령, 업무, 자격증 취득 등의 기능이 다르게 처리되어야 한다.
