# 필수 정리 내용
## IoC와 프레임워크, 라이브러리
### IoC
`IoC`는 Inversion of Control의 줄임말로 제어의 역전이다. 제어의 역전은 무엇일까? 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부, 즉 프레임워크에서 관리하는 것을 말한다. 그렇기 때문에 IoC는 DI와도 밀접한 관련이 있다.

### 프레임워크 VS 라이브러리
- 내가 작성한 코드를 제어하고 대신 실행하면 프레임워크
- 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 라이브러리

## 생성자를 통한 의존성 주입과 @Autowired 비교
`@Autowired` 어노테이션을 통한 의존성 주입은 필드 주입이다. `@Autowired` 어노테이션을 통한 필드 주입은 코드가 간결해서 좋아 보이지만 외부에서 변경이 불가능하다는 단점이 있다. 그래서 테스트를 하기 힘들다. 하지만 생성자를 통한 의존성 주입은 생성자 호출시점에 딱 1번만 호출되는 것이 보장된다. 

다만, 생성자에 @Autowired 어노테이션을 붙여서 생성자 주입을 할 수도 있지만, 최근에 롬복에서는 `@RequiredArgsConstructor` 어노테이션을 지원해 final이 붙은 필드를 모아서 생성자를 자동으로 만들어준다. 

## AOP
`AOP`는 Aspect-Oriented Programming의 약자로 관점 지향 프로그래밍이라 불린다. DI가 의존성에 대한 주입이라면, AOP는 로직 주입이라고 할 수 있다. 
코드는 핵심 관심사와 횡단 관심사로 이루어져 있는데, 핵심 관심사는 모듈별로 다르지만 횡단 관심사는 모듈별로 반복되어 중복해서 나타나는 부분이다. 이 부분은 **Aspect로 모듈화하고 핵심 관심사와 분리해서 재사용하는 것**이 `AOP`가 하는 역할이다. 

- `Aspect`: 여러 개의 Advice와 여러 개의 Pointcut의 결합체
- `Advisor`: 다수의 Advice와 다수의 Pointcut만ㅇ르 결합한 것, Aspect가 나왔기 때문에 사용할 필요가 없음.
- `Advice`: Pointcut에 언제, 무엇을 적용할지 정의한 메서드
- `JoinPoint`: Aspect 적용이 가능한 모든 지점, 더 구체적으로 말하면 스프링 프레임워크가 관리하는 빈의 모든 메서드를 말한다.
- `Pointcut`: 횡단 관심사를 적용할 타깃 메서드를 선택하는 지시자(JoinPoint의 부분 집합)

# 선택 정리 내용
## 1. 보기처럼 의존성 주입할 때, 다음 질문에 답하시오.
### (1) 의존성 주입에는 어떤 것이 있는지 기재하시오.
의존성 주입에는 3가지 종류가 있다. 생성자를 통해서 의존성을 주입하는 생성자 주입, Setter를 통해서 의존성을 주입하는 수정자 주입, 필드에서 주입하는 필드 주입이 있다.

### (2) 여러 의존성 주입 방법중에 private final 문법과 @RequiredArgsConstructor 어노테이션을 사용하여 주입 하는 이점이 무엇일지 쓰시오.
수정자 주입을 통해서 의존성을 주입하면 메서드가 public으로 열려있기 때문에 변경될 가능성이 있지만, 생성자 주입은 객체를 생성할 때 딱 1번만 호출하면 되므로 애플리케이션이 종료될 때까지 의존 관계를 변경할 일이 없다.
또한, 필드에 `final` 키워드를 사용하기 때문에 의존성 주입이 누락되어 `NullPointException`을 해결해준다.

### (3) @AllArgsConstructor, @NoArgsConstructor는 무엇인지 쓰시오.
#### @AllArgsConstructor
`@AllArgsConstructor` 어노테이션은 모든 필드값을 파라미터로 받는 생성자를 만든다. 

```java
public ProductService(UserUtils userUtils, UserRepository userRepository, ..., AlarmService alarmService) {
  this.userUtils = userUtils;
  this.userRepository = userRepository;
  ...
  this alarmService = alarmService;
}
```
### @NoArgsConstructor
`@NoArgsConstructor` 어노테이션은 기본 생성자를 생성해준다. JPA는 Entity 객체를 인스턴스화하고 필드에 값을 채워넣기 위해 Reflection을 사용해 런타임 시점에 동적으로 기본 생성자를 통해 클래스를 인스턴스화하여 값을 매핑하기 때문에 기본 생성자를 만들어야 한다.
이 때, Entity Class에 기본 생성자가 존재하지 않는다면 JPA는 Entity 객체를 동적으로 인스턴스화 할 수 없으므로 JPA의 기능을 원활하게 사용하지 못한다.
