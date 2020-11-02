---
title: "Spring Annotation"
category: Web, Spring
---

# 어노테이션



# Spring Annotation

- 주석이라는 사전적 의미가 있다.
- 컴파일 혹은 런타임에 해석된다.
- @를 이용한 주석, 자바코드에 주석을 달아 특별한 의미를 부여한 것
- 메타데이터(실제데이터가 아닌 Data를 위한 데이터)라고 불리고 JDK5부터 등장
- 기존에는 XML로 컨텍스트들을 관리하였는데 XML이 너무 많아지면 유지보수성이 낮아진다. 
- 그래서 설계시 확정되는 부분은 Annotation 기반 설정으로 개발의 생산성을 높인다.



## 자주 쓰는 Annotation 정리

> 의존성 주입용도

@ComponentScan

- @Component와 @Service, @Respository, @Controller, @Configuration 이 붙은 클래스 Bean들을 찾아서 Context에 Bean등록을 해주는 어노테이션
- 어노테이션을 사용하지 않는다면 ApplicationContext.xml에 Bean을 직접 등록해야 한다.



@Required

- Optional 하지 않은, 꼭 필요한 속성들을 정의한다.
- Setter메소드에 적용해주면 빈 생성시 필수 프로퍼티 임을 알려준다.



@Autowired

- Field(속성), Setter Method, Constructor(생성자)에서 사용하다. 
- 스프링이 IoC 컨테이너 안에 존재하는 Bean을 자동으로 주입한다.



@ Qualifier

- 같은 타입의 Bean이 두개 이상이 존재하는 경우에 스프링이 어떤 Bean을 주입해야 할지 모르기 때문에 스프링 컨테이너를 초기화하는 과정에서 예외가 발생된다.
- 이러한 경우 @Qualifier을 @Autowired와 함께 사용하여 정확히 어떤 Bean을 사용할지 지정하여 특정 객체를 주입할 수 있도록 한다.



@Inject

- Autowired 어노테이션과 비슷한 역할을 한다. 
- 특정 프레임워크에 종속되지 않은 어플리케이션을 구성하기 위해서 @Inject를 사용할 것을 권장



> 컨트롤러 관련

@Controller

- 스프링 프레임워크에서 Controller 객체라는 것을 알려준다.
- API와 View를 동시에 사용하는 경우에 사용
- API 서비스로 사용하는 경우에는 @ResponseBody 를 사용하여 객체 반환
- View(화면) return 이 주된 목적



@RestController

- View가 필요없는 API만 지원하는 서비스에서 사용
- data(json, xml 등) return 이 주된 목적
- @RestController = @Controller + @ResponseBody 



@RequestMapping

- Controller 객체 안에 있는 메소드와 클래스에 사용하는 어노테이션
- URI 요청이 RequestMapping(value="")에 value값이 일치하면 해당 클래스나 메소드가 실행된다.



@PathVariable

- 메소드 파라미터 앞에 사용하면 해당 URL에서 {특정값}을 변수로 받아 올 수 있다.



@RequestBody

- 요청 문자열이 그대로 파라미터로 전달되도록 하는 어노테이션
- 요청이 온 데이터(JSON, XML, ..)를 바로 클래스나 Model로 매핑하기 위한 어노테이션



@RequestParam

- 화면단에서 요청을 통해 넘어온 값을 Controller에서 받을 때 @RequestMapping으로 명시된 메소드의 파라미터에 @RequestParam을 사용한다.
- 요청에서 특정한 파라미터 값을 찾아낼 때 사용하는 어노테이션



@ResponseBody

- 메소드와 리턴타입에 사용
- 리턴 타입이 HTTP의 응답 메시지로 전송이 되도록 하는 어노테이션



> 데이터 접근 관련

@Service

- DAO 클래스에서 사용
- DAO 객체임을 알려주는 어노테이션



@Repository

- 비지니스 서비스, 서비스 레이어, 비지니스 로직이 들어가는 Service로 등록
- Service 클래스에서 사용
- Service 객체임을 알려주는 어노테이션





