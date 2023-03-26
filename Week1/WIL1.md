# MVC 패턴
Model, View, Controller의 줄임말로 View는 화면을 그리는 일에 모든 역량을 집중하고 Controller와 Model은 비지니스 로직과 관련이 있거나 내부적인 것을 처리하는 일에 집중한다. helloMvc 메서드를 통해서 MVC 패턴을 이해해보자.

```java
@GetMapping("hello-mvc")
public String helloMvc(@RequestParam("name") String name, Model model) {
    model.addAttribute("name", name);
    return "hello-template";
}
```

- URL에 있는 hello-mvc를 매칭시켜주기 위해서 @Getmapping이 hello-mvc로 되어 있는 Controller를 찾은 후 메서드를 실행해준다.
- @RequestParam 어노테이션은 사용자가 전달하는 값을 매핑시켜준다. 입력값을 name 변수에 저장한다. 
- Model 객체는 컨트롤러에서 데이터를 생성해 View에 전달하는 역할을 하고, key와 value를 담고 있다. addattribute(key, value) 메서드를 사용하여 전달할 데이터를 매핑하여 hello-template을 리턴하면 viewResolver가 리턴된 문자에 대응하는 templates의 html 파일을 찾고 렌더링 시킨다. html 파일에서 p태그의 ${name} 부분을 value로 치환해준다.

# API
key와 value로 이루어진 Json의 데이터 구조 포맷의 형태로 클라이언트에게 데이터를 전달하는 방식을 말한다. @ResponseBody를 통해서 반환값을 그대로 클라이언트에게 전달한다.@ResponseBody를 사용하면 viewResolver대신에 HttpMessageConverter가 동작한다. @ResponseBody를 가진 메서드가 반환하는 데이터가 문자이면 StringHttpMessageConverter, 객체이면 MappingJackson2HttpMessageConverter가 동작한다.

## 문자 반환
- @ResponseBody를 사용하면 HTTP의 BODY에 문자 내용을 그대로 반환한다.
- 마우스 오른쪽을 눌러 소스 보기를 하면 소스 코드가 나오는 것이 아니라 결과 문자가 나온다는 것이 @ResponseBody의 특징이다.

## 객체 반환
- 우리가 일반적으로 말하는 API는 객체를 반환하는 것이다. 
- Json의 형태(key: value)로 결과가 나온다.

# RESTful
RESTful은 REST에 대한 원칙을 모두 준수하는 시스템을 의미한다. RESTful을 이해하기 위해서 REST가 무엇인지 알아보자.

## REST
REST는 RE(Representational), S(Status), T(Transfer)의 줄임말로, 자원(리소스)의 표현에 의한 상태(정보)를 전달하는 것을 의미한다. 말이 너무 어려운데, REST는 HTTP를 잘 활용하기 위한 원칙 또는 네트워크 상에서 클라이언트와 서버 사이의 통신 방식이라고 알고 있으면 된다. 그렇다면 REST는 HTTP를 어떻게 활용할까?

>자원(리소스): HTTP URI
상태(정보) 전달: HTTP Method

## REST가 필요한 이유
- 애플리케이션 분리 및 통합
- 다양한 클라이언트의 등장

## REST 제약
- Uniform interface: 전체적인 시스템 아키텍처를 간단하고 잘 파악할 수 있도록 하기 위한 약속된 인터페이스이다.
-  Client–server: 클라이언트와 서버의 역할을 구분하여 의존성을 줄인다.
- Stateless: REST는 HTTP의 특성을 이용하기 때문에 무상태성을 갖는다.
- Cacheable: 요청에 대한 응답 내의 데이터에 해당 요청은 캐시가 가능한지 불가능 한지 명시해야 한다.
- Layered system: 리소스 상태를 요청하는 클라이언트와 응답하는 서버 사이에 여러 개의 중간 서버가 존재한다.
- Code on demand(optional): 클라이언트는 리소스에 대한 표현을 응답으로 받고 처리해야 하는데, 어떻게 처리해야 하는지에 대한 Code를 서버가 제공하는 것을 의미한다.

다음 4가지의 HTTP 메서드를 가지고 CRUD를 할 수 있다.

>Post: C(Create)
Get: R(Read)
Put/Patch: U(Update)
Delete: D(Delete)

## RESTful의 목적
- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것이다.
- RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.
