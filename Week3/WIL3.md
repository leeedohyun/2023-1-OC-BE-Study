# 필수 정리 내용
## 의존과 의존성
MemberService를 보면 MemberRepository를 사용하고 있다. 이러한 관계를 MemberService가 MemberRepository에 의존한다고 표현한다. 

## @Autowired 의존성 주입
생성자에 @Autowired 가 있으면 스프링이 연관된 객체를 스프링 컨테이너에서 찾아서 넣어준다. 이렇게 객체 의존 관계를 외부에서 넣어주는 것을 DI (Dependency Injection), 의존성 주입이라 한다. DI의 방식은 3가지가 있다.

### 필드 주입
생성자를 제거하고 @Autowired를 필드에 의존 관계를 주입하는 방식으로 별로 좋지는 않다.
```java
 @Controller
 public class MemberController {
 	
    @Autowired private MemberService memberService;
 }

```

### setter 주입
setter를 통해서 의존 관계를주입하는 방식이다. 단점은 누군가가 멤버 컨트롤러를 호출할 때 set 메서드가 public으로 열려 있어야 하고 변경이 쉽기 때문에 잘못 바꿨을 때 문제가 발생할 수 있다는 것이다.

```java
 @Controller
 public class MemberController {
 
 	private MemberService memberService;
    
    @Autowired
    public void setMemberService(MemberService memberService) {
    	this.memberService = memberService;
    }
 }

```

### 생성자 주입
생성자를 통해서 의존 관계를 주입시키는 방식으로 의존 관계가 실행 중에 동적으로 변하는 경우는 거의 없으므로 가장 권장하는 방식이다.

```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}

```

## DIP
SOLID 원칙 중의 하나로 Dependency Inversion principle, 의존 관계 역전 법칙이다. 이 원칙은 프로그래머는 구체화에 의존하면 안 되고 추상화에 의존해야 하는 것을 의미한다. 즉, 구현 클래스에 의존하지 말고 인터페이스에 의존하라는 뜻이다.

## 스프링 빈과 스프링 컨테이너
간단하게 정리하자면, 

### 스프링 컨테이너
- 자바 객체를 관리하는 공간
- DI를 적용하는 공간

### 스프링 빈
- 빈은 자바의 객체를 의미한다.

### 스프링 빈으로 등록하는 2가지 방법
1. 컴포넌트 스캔 원리: @Component 어노테이션을 통해서 자동으로 스프링 빈으로 등록한다. - @Controller, @Service, @Repository 어노테이션은 @Component를 포함하고 있어서 스프링 빈으로 자동 등록된다.
2. 자바 코드로 직접 등록: 위의 어노테이션을 사용하지 않고 직접 자바 코드를 작성하여 빈을 등록한다.


## JDBC
JDBC는 데이터베이스에 접근할 수 있도록 하는 오래된 방식이다. 

## JPA
Java Persistence API로 자바 진영에서 ORM(Object-Relational Mapping)의 기술 표준으로 사용하는 인터페이스의 모음이다. JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행하기 때문에 개발 생산성을 크게 높일 수 있다. 또한 JPA를 사용하면 SQL과 데이터 중심의 설계에서 **객체 중심의 설계**로 패러다임을 전환을 할 수 있다. JPA를 통해서 개발 생산성을 높였지만 스프링 데이터 JPA를 사용하면 리포지토리에 구현 클래스 없이 인터페이스로만 개발을 완료할 수 있다. 그리고 반복 개발해 온 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공한다.

# 선택 정리 내용
## 1.`GetMapping(“hello”)의 의미는 무엇인가? 해당 화면을 보고 싶으면 어떤 url로 접속하면 되는가?
url에 hello를 매칭시켜주기 위해서 사용한다. 이 화면을 보기 위해서 localhost:8080/hello로 접속하면 된다.

## 2. `model`은 어떤 용도로 쓰이는가?
1. 모델을 굳이 선언해서 사용하는 이유: 데이터를 뷰로 보내주는 역할을 하기 때문이다.
2. “data”를 html 파일에서 사용하는 방법: addAttribute 메서드를 통해서 key와 value를 받는다. 그리고 나서 ${data} 부분을 value인 "hello!!"로 치환해준다.

## 3. 아래 코드에서 `return “hello”;`를 하는 이유는 무엇인가?
viewResolver가 리턴된 문자에 대응하는 templates의 html 파일을 찾고 렌더링 시킨다.

## 4. 만약 `GetMapping`밑에 `@ResponseBody`를 삽입한다면  `localhost:8080/hello-string`에 접속 시 무엇이 출력 될 것인가?
오류가 발생하여 오류 페이지가 나온다. @RequestParam을 통해서 name을 직접 입력해주어야 한다.

## 5. JPA란?
1. JPA를 굳이 왜 쓰는가?
- 기존의 반복 코드는 물론이고, 기본적인 CRUD SQL도 JPA가 직접 만들어서 실행하기 때문에 개발 생산성을 크게 높일 수 있다.  - SQL과 데이터 중심의 설계에서 **객체 중심의 설계**로 패러다임을 전환을 할 수 있다. 
2.  jpql이란? 
- sql과 문법이 비슷한 엔티티 객체를 조회하는 **객체 지향 쿼리**이다.
3. SQL문 select * from user where id = 1을 jpql로 변환한 결과를 쓰시오.
- select * from User u where u.id=1

## 6. 회원 리스트 html에서 아래 사진의 실행 결과를 출력해서 붙여넣기 하시오.
<img width="200" alt="스크린샷 2023-01-17 오후 2 08 53" src="https://user-images.githubusercontent.com/116694226/229833140-306513f7-b572-4bce-a658-d93e6a3b2046.png">


## 7
(1) @Controller <br>
(2) @Repository
