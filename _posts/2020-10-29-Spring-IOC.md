---
title: "Spring IOC"
category: Web, Spring
---



### 1. 컨테이너

- 인스턴스의 생명주기를 관리
- 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는것
- 즉, 컨테이너는 객체의 생성과 소멸을 컨트롤 해준다.



# IOC/DI 컨테이너?

스프링 프레임워크에서 객체에 대한 생성 및 생명주기를 관리

POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가진다.

![spring](https://user-images.githubusercontent.com/23491962/97589426-d218c580-1a40-11eb-8a39-8b527db75eff.JPG)


# POJO?

- Plain Old Java Object
- 주로 특정 모델이나 기능, 프레임워크를 따르지 않는 Java Object를 지칭



### 2. 컨테이너 종류

스프링 컨테이너의 종류는 두가지로 나누어진다.

# 1) BeanFactory

- Bean을 등록, 생성, 조회, 반환 관리한다.
- 팩토리 디자인 패턴으로 구현된 것
- BeanFactory는 빈을 생성하고 분배하는 책임을 지는 클래스
- BeanFactory가 Bean의 정의는 즉시 로딩하는 반면, Bean 자체가 필요하기 전까지는 인스턴스화를 하지 않는다.
- Bean을 조회할 수 있는 getBean() 메소드가 정의되어 있다.

# 2) ApplicationContext

- BeanFatory와 유사한 기능을 제공하지만, 좀 더 많은 기능을 제공한다.
- Bean을 등록, 생성, 조회, 반환 관리 기능은 BeanFactory와 같다.
- 국제화가 지원되는 텍스트 메시지를 관리
- 이미지같은 파일 자원을 로드할 수 있는 포괄적인 방법을 제공
- 리스너로 등록된 빈에게 이벤트 발생을 알려준다.


BeanFactoy 와 ApplicationContext 차이점

- BeanFactory : 처음으로 getBean()이 호출되어야 해당 빈을 생성(Lazy Loading, 게으른 호출)
- ApplicationContext : 컨텍스트 초기화 시점에 모든 빈을 미리 로드하여 Bean을 지연없이 얻을 수 있다.




### 3. IOC

- Inversion Of Control : 제어의 역전
- 객체의 생성, 생명주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 의미
- 스프링을 쓰기전에는 개발자가 코드를 제어하는 주체
- 스프링에서는 애플리케이션 코드를 프레임워크가 주도한다.(@Autowired 등으로 Bean을 자동으로 주입)
- 제어권이 컨테이너로 넘어옴으로써 DI와 AOP등이 가능하게 되었다.
