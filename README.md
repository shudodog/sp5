# sp5
2021.08
****스프링5 프로그래밍 입문 (최범균 저)****

스프링의 기초와 전반적인 흐름을 배웠다. 스프링 기본적인 사용법을 다루었고, DB 연동(JDBC) 그리고 스프링 MVC에 대해 배웠다.
특히 Chapter10 스프링 MVC 프레임워크 동작 방식에서 많은 것을 배웠는데, 이 책을 읽기 전까지는 스프링 MVC 핵심 구성요소와 각 요소 간의 관계를 
명확히 알지 못하였다. 하지만 책을 읽고 난 후 스프링 MVC의 흐름을 이해할 수 있었다.
그리고 가장 어려웠던 부분은 Chapter7 AOP 프로그래밍 이었다. 처음 읽었을 때 잘 이해가 가지 않아 먼저 다음으로 넘어간 후 책을 2번째 읽을 때 더욱 꼼꼼히 봤던 부분이다.

****Chapter7 AOP 프로그래밍****
프록시는 핵심 기능은 구현하지 않고 대신 여러 객체에 공통으로 적용할 수 있는 기능을 구현한다.
AOP(Aspect Oriented Programming)의 핵심은 공통 기능 구현과 핵심 기능 구현을 분리하는 것이다. AOP는 핵심기능을 구현한 코드의 수정 없이 공통 기능을 적용 할 수 있게 만들어준다.
기본 개념은 핵심 기능에 공통 기능을 삽입하는 것이다. 스프링에서는 런타임에 프록시 객체를 생성해서 공통 기능을 삽입하는 방법을 사용하고 있다.

스프링 AOP구현 방법
  Aspect로 사용할 클래스에 @Aspect 어노테이션을 붙인다.
  @Pointcut 어노테이션으로 공통 긴으을 적용할 Pointcut정의
  공통 기능을 구현한 매서드에 @Around 어노테이션을 적용한다.
![image](https://user-images.githubusercontent.com/76150392/130768990-53d8be27-d04c-43f2-a846-4bab1f92d7a2.png)

****Chapter10****
1.DispatcherServlet은 모든 연결을 담당한다. 웹 브라우저로부터 요청이 들어오면 DispatcherServlet은 그 요청을 처리하기 위한 컨트롤러 객체를 검색한다.

2.이때 DispatcherServlet은 직접 컨트롤러를 검색하지 않고 HandlerMapping이라는 빈 객체에게 컨트롤러 검색을 요청한다. HandlerMapping은 클라이언트의 요청 경로를 이용해서 이를 처리할 컨트롤러 빈 객체를 DispatcherServlet에 전달한다. 예를 들어 웹 요청 경로가 '/hello'라면 등록된 컨트롤러 빈 중에서 '/hello'요청 경로를 처리할 컨트롤러를 리턴한다.

3.DispatcherServlet은 @Controller, Contoller 인터페이스, HttpRequestHAndler 인터페이스를 구현한 클래스를 동일한 방식으로 처리하기 위해 HandlerMapping이 찾아준 컨트롤러 객처를 처리할 수 있는 HandlerAdapter빈에게 요청 처리를 위임한다.

4-5.HandlerAdapter는 컨트롤러의 알맞은 메서드를 호출해서 요청을 처리한다.

6.그 결과를 DispatcherServlet에 리턴한다.

7.HandlerAdapter로부터 ModelAndView로 요청 처리 결과를 받고 DispatcherServlet은 결과를 보여줄 뷰를 찾기 위해 ViewResolver빈 객체를 사용한다. ModelAndView가 컨트롤러가 리턴한 뷰 이름을 가지고 있는데 ViewResolver는 View객체를 찾거나 생성해서 리던한다. 응답을 생성하기 위해 JSP를 사용하는 VIewResolver는 매번 새로운 View 객체를 생성해서 DispatherServlet에 리턴한다.

8.DispatcherServlet은 ViewResolver가 리턴한 View객체에게 응답 결과 생성을 요청한다.

@Controller를 위한 HandlerMapping과 HandlerAdapter
클라이언트의 요청을 실제로 처리하는 것은 컨트롤러이고 DispatcherServlet은 클라이언트의 요청을 전달받는 창구 역할을 한다. DispatcherSErvlet은 클라이언트의 요청을 처리할 컨트롤러를 찾기 위해 HandlerMapping을 사용하는데, @Controller 애노테이션을 붙인 클래스를 이용할 수 도 있지만, 자신이 직접 만든 클래스를 이용해 클라이언트의 요청을 처리할 수도 있다.

HandlerMapping이나 HandlerAdapter 클래스를 빈으로 등록하는 대신 @EnableWebMvc 애노테이션만 추가하면 된다.
![image](https://user-images.githubusercontent.com/76150392/130773655-ca0dfa7f-bedc-40f5-8a8a-27c33d6a63d7.png)

RequestMappingHandlerAdapter 애노테이션은 컨트롤러의 메서드를 알맞게 실행하고 그 결과를 ModelAndView 객체로 변환해서 DispatcherServlet에 리턴한다.
![image](https://user-images.githubusercontent.com/76150392/130773843-8a2c2a2c-a5df-46ce-8ef3-ac1924befd0a.png)

RequestMappingHandlerAdapter 클래스는 "/hello"요청 경로에 대해 hello()매서드를 호철한다. 이 때 Model 객체를 생성해서 첫 번째 파라미터로 전달한다. 비슷하게 이름이 "name"인 HTTP요청 파라미터의 값을 두 번째 파라미터로 전달한다. RequestMappingHandlerAdapter는 컨트롤러 메서드 결과 값이 String 타입이면 해당 값을 뷰 이름으로 갖는 ModelAndView객체를 생성해서 DispatcherServlet에 리턴한다. 이때 첫 번째 파라미터로 전달한 Model 객체에 보관된 값도 ModelAndView에 함께 전달한다. 예제코드는 "hello"를 리턴하므로 뷰 이름으로 "hello"를 사용한다.

JSP를 위한 ViewResolver
DispatcherServlet은 컨트롤러의 실행결과를 HandlerAdapter를 통해서 ModelAndView 형태로 받는다고 했다. Model에 담긴 값은 View 객체에 Map 형식으로 전달된다. 예를 들어 HelloController클래스는 다음과 같이 Model에 "greeting" 속성을 설정했다.
![image](https://user-images.githubusercontent.com/76150392/130774890-c6e9cfd7-d805-4d37-84e3-5fa749339f48.png)

이 경우 DispatcherServlet은 View객체에 응답 생성을 요청할 때 greeting 키를 갖는 Map 객체를 View 객체에 전달한다. View 객체는 전달받은 Map 객체에 담긴 값을 이용해 알맞은 응답 결과를 출력한다. InternalResourceView는 Map객체에 담겨 있는 키 값을 request.setAttribute()를 이용해서 request의 속성에 저장한다. 그 뒤 해당 경로의 JSP를 실핸한다.

결과적으로 컨트롤러에서 지정한 Model 속성은 request 객체 속성으로 JSP에 전달되기 때문에 JSP느 ㄴ다음과 같이 모델에 지정한 속성 이름을 사용해서 값을 사용할 수 있다.
![image](https://user-images.githubusercontent.com/76150392/130775326-d19789c1-5cf2-41b9-8689-2d0dbb7934f1.png)



