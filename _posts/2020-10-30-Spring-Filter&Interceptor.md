---
title: "Spring Filter & Interceptor"
category: Web, Spring
---





- **공통 프로세스**에 대한 고민

웹 개발을 하다 보면, 공통적으로 처리해야 할 업무들이 있다.

예를 들어 *'로그인 관련', '권한 체크', 'XSS 방어', 'PC와 Mobile의 분기처리', '로그 확인', '페이지 인코딩 변환'* 등이 있다.

이러한 공통 업무에 관련된 코드를 모든 페이지마다 작성한다면 중복된 코드가 많아지게 된다.

그래서 <u>공통 부분은 빼서 따로 관리</u>하는게 좋고 이를 위해 활용할 수 있는 방법이 3가지가 있다.

1. Filter
2. Interceptor
3. AOP



위 세가지 기능은 어떤 기능을 수행하기 전이나 수행한 후에 추가적으로 처리를 할 때 사용되는 기능들이다.





## Filter, Interceptor, AOP의 흐름

- Filter와 Interceptor는 Servlet 단위에서 실행된다.
- AOP는 메소드 앞에 Proxy 패턴의 형태로 실행된다.
- 실행 순서를 보면 Filter가 가장 밖에 있고, 그 안에 Interceptor, 그리고 그 안에 AOP가 있다.
![spring-filter](https://user-images.githubusercontent.com/23491962/97719281-3bb0d680-1b0a-11eb-8243-f1c22385718c.JPG)




## Filter, Interceptor, AOP의 개념

1. Filter

   - Servlet Filter는 DispatcherServlet 이전에 실행이 되기 때문에 앞단에서 요청내용을 변경하거나 여러가지 체크를 할 수 있다.
   - 또한 자원의 처리가 끝난 후 응답내용에 대해서도 변경하는 처리를 할 수 있다.
   - web.xml에 등록한다.
   - 주로 '인코딩 변환', '로그인 여부 확인', '권한체크', 'XSS 방어' 등에 사용한다.
   - init() : 필터 인스턴스 초기화
   - doFilter() : 전/후 처리
   - destroy() : 필터 인스턴스 종료

   

2. Interceptor

   - 스프링은 DispathcerServlet으로부터 시작이 되므로 Filter는 스프링 컨텍스트 외부에 존재한다.
   - 하지만 Interceptor는 스프링의 DispathcerServlet이 컨트롤러를 호출하기 전,후로 끼어들기 때문에 스프링 컨텍스트 내부에 존재한다.
   - 스프링 내의 모든 객체(Bean) 접근이 가능하다.
   - '로그인 체크', '권한체크', '로그확인', '업로드 파일처리' 등에 사용한다.
   - preHandler() : 컨트롤러 메소드가 실행되기  전
   - postHandler() : 컨트롤러 메소드 실행직후 view페이지가 렌더링 되기 전
   - afterCompletion() : view페이지가 렌더링 된 후

   

3. AOP

   - OOP를 보완하기 위해 나온 개념
   - 기존의 OOP로직의 흐름은 계정, 게시판, 계좌이체를 처리할 때마다 권한, 트랜잭션, 로깅을 처리해야하기 때문에 모든 로직에서 똑같은 코드가 반복
   - 이러한 중복을 줄이기 위해 종단면(관점)에서 바라보고 처리
   - 주로 '로깅', '트랜잭션', '에러처리' 등 비지니스단의 메소드에서 사용
   - Filter와 Interceptor와 달리 메소드 전후의 지점에 자유럽게 설정이 가능
   - AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터 차이
     - Advice의 경우 JoinPoint나 ProceedingJoinPoint 등을 호출
     - HandlerInterceptor는 Filter와 유사하게 HttpServletRequest, HttpServletResponse를 파라미터로 사용
   - AOP 포인트컷
     - @Before : 대상 메소드의 수행 전
     - @After : 대상 메소드의 수행 후
     - @After-returning : 대상 메소드의 정상적인 수행 후
     - @After-throwing : 예외발생 후
     - @Around : 대상 메소드의 수행 전,후





## 정리

|    구분     | Filter                        | Interceptor                                    | AOP                                               |
| :---------: | ----------------------------- | ---------------------------------------------- | ------------------------------------------------- |
|  실행 위치  | Servlet                       | Servlet                                        | Method                                            |
|  실행 순서  | 1                             | 2                                              | 3                                                 |
|  설정 위치  | web.xml                       | xml or java                                    | xml or java                                       |
| 실행 메소드 | init(), doFilter(), destroy() | preHandler(), postHandler(), afterCompletion() | 포인트컷으로 @After, @Before, @Around 위치를 지정 |

