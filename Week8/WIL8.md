# 필수 정리 내용
## IoC와 프레임워크, 라이브러리
### IoC
`IoC`는 Inversion of Control의 줄임말로 제어의 역전이다. 제어의 역전은 무엇일까? 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부즉 프레임워크에서 관리하는 것을 말한다.

### 프레임워크 VS 라이브러리
- 내가 작성한 코드를 제어하고 대신 실행하면 프레임워크
- 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 라이브러리

## 생성자를 통한 의존성 주입과 @Autowired 비교

## AOP

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

## 2. AOP를 이용하여 많은 기능을 구현할 수 있다. 대표적인 것을 조사하고 정리하시오.
