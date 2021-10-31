**클린 코드 이전에 클린 시스템 !**

## 시스템 제작과 사용을 분리하라

### 초기화 지연 기법 ≠ 시스템 생성 사용 분리 기법

```java
public Service getService() {
	if (service == null)
		service = nuw MySeviceImp(...);
	return service;
}
```

- 단일 책임 원칙(Single Responsibility Principle, SRP) 을 깸
- 초기화 지연 기법은 정말 초기화가 큰 비용을 발생 시키는 경우에만 사용
- 테스트 비용, 시스템의 복잡도 상승으로 되려 큰 비용이 발생함

### 생성 사용의 분리 기법

- **main 분리**
  - 생성과 관련한 것들을 main 으로 이동
  - 나머지 시스템은 모든 객체가 생성 되었고 의존성이 연결 되었다고 가정
  - 생성 뒤 이를 애플리케이션에 넘기는 방식
- **팩토리**
  - 객체의 생성 시기를 직접 결정할 필요도 존재
  - main 에서는 완성된 객체가 아닌 팩토리 객체를 만들어 생성 시점 또한 제어할 수 있음
- **의존성 주입** (DI, Dependency Injection)
  - 아주 강력한 사용 제작 분리 기법 메커니즘
  - 단일 책임 원칙 (Single Responsibility Principle, SRP) 를 지키기에 유용
    - 객체는 의존성 자체를 인스턴스로 만드는 생성의 책임을 지지 않음
    - 이런 책임은 다른 전담 메커니즘에 넘겨 제어를 넘김
  - 초기화 지연으로 얻는 장점 또한 포기 하지 않아도 됨
    - DI 컨테이너는 대부분 필요할 때까지 객체를 생성하지 않음

## 확장

### **관심사를 적절히 분리해 관리하라**

- '처음부터 올바르게' 시스템을 만들 수 있다는 생각은 미신
- 깨끗한 시스템 아키텍쳐는 테스트 주도 개발, 리팩터링, 깨끗한 코드는 코드 수준에서 시스템을 조정하고 확장하기 쉽게 함

### 횡단 (cross-cutting) 관심사

- 이론적으로 독립된 형태로 구분 가능
- 실제로는 코드에 잔재하기 쉬운 부분들 ( 트랜잭션, 인증/인가, 로깅 등)
- 관심사로 명확히 분리하고, 특정 지점들이 동작하는 방식을 일관성 있게 변경하라

### 활용 가능한 적절한 관심사 분리 기법

- 자바 프록시
  - 프록시(Proxy) 란?
    - 임차 대리인처럼 대리 객체를 통해 구현
    - 인터페이스를 구현 ⇒ 프록시 객체로 구현 ⇒ 실제 작업을 수행할 때 실제 객체에 넘겨주는 방식
  - 간단한 경우 자바 프록시가 적절
  - java.lang.reflect 과 같은 프레임워크를 통해 자바 프록시 구현 가능
  - 프록시 클래스를 통해 구현
    - 비즈니스 로직과 분리된 객체들을 재생산 해서 더욱 많은 부분을 점검 해야 할 수도 있음
- 순수 자바 AOP (Aspect-Oriented programmming, 관점 지향 프로그래밍) 프레임워크
  - POJO (Plain Old Java Object)
    - 공통적인 문제, 모듈을 관리하는 데에 초점
    - 자신이 속한 도메인에 초점이 맞추어진다.
    - 테스트가 간결해짐
- AspectJ
  - 매우 강력하지만 그만큼 새 도구 사용에 있어 문법과 사용법을 익혀야 한다는 단점이 존재
  - AspectJ annotation 을 통해 AOP 지원

## 의사 결정을 미루라

- 때때로 가능한 마지막 순간까지 결정을 미루는 방법이 최선
- 관심사를 모듈로 매우 빠르게 분리하지 마라
- 천천히 피드백을 모으고 고민해 구현 방안을 끝까지 탐험하라

## 표준을 표준답게

- 완벽하고 견고한 시스템은 반드시 표준을 사용한 것이 아니다
- 충분한 시간적 여유가 있는 프로젝트는 없다. 대신 효율적으로 시스템을 설계하라

## 시스템은 도메인 특화 언어가 필요

- 좋은 DSL 은 도메인 개념과 그 개념을 구현한 코드 사이에 존재하는 의사소통의 간극을 줄인다.
- 효과적으로 도메인 특화 언어 (Domain-Specific Language, DSL) 를 사용하면 코드 추상화 수준을 관용구나 디자인 패턴 이상으로 끌어올린다.
- 도메인 언어처럼 시스템 또한 적절한 관심사 분리, 높은 추상화 수준으로 유지하라