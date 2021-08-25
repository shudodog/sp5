# sp5
****스프링5 프로그래밍 입문 (최범균 저)****

스프링의 기초와 전반적인 흐름을 배웠다. 스프링 기본적인 사용법을 다루었고, DB 연동(JDBC) 그리고 스프링 MVC에 대해 배웠다.
특히 Chapter10 스프링 MVC 프레임워크 동작 방식에서 많은 것을 배웠는데, 이 책을 읽기 전까지는 스프링 MVC 핵심 구성요소와 각 요소 간의 관계를 
명확히 알지 못하였다. 하지만 책을 읽고 난 후 스프링 MVC의 흐름을 이해할 수 있었다.
그리고 가장 어려웠던 부분은 Chapter7 AOP 프로그래밍 이었다. 처음 읽었을 때 잘 이해가 가지 않아 먼저 다음으로 넘어간 후 책을 2번째 읽을 때 더욱 꼼꼼히 봤던 부분이다.

****Chapter7 AOP 프로그래밍****
프록시는 핵심 기능은 구현하지 않고 대신 여러 객체에 공통으로 적용할 수 있는 기능을 구현한다.
AOP(Aspect Oriented Programming)의 핵심은 공통 기능 구현과 핵심 기능 구현을 분리하는 것이다. AOP는 핵심기능을 구현한 코드의 수정 없이 공통 기능을 적용 할 수 있게 만들어준다.
기본 개념은 핵심 기능에 공통 기능을 삽입하는 것이다. 스프링에서는 런타임에 프록시 객체를 생성해서 공통 기능을 삽입하는 방법을 사용하고 있다.
![image](https://user-images.githubusercontent.com/76150392/130768990-53d8be27-d04c-43f2-a846-4bab1f92d7a2.png)

스프링 AOP구현 방법
  Aspect로 사용할 클래스에 @Aspect 어노테이션을 붙인다.
  @Pointcut 어노테이션으로 공통 긴으을 적용할 Pointcut정의
  공통 기능을 구현한 매서드에 @Around 어노테이션을 적용한다.

****Chapter10 
