---
title: "Design Pattern : Singleton pattern"
category: Design Pattern
---



# Singleton Pattern

- 특정 클래스에 대해서 **객체 인스턴스가 하나만 만들어질** 수 있도록 하고, **어디에서든지 그 인스턴스에 참조할 수** 있도록 하는 패턴
- 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초에 생성한 객체를 반환

```java
Public class Singleton {
	private static Singleton uniqueInstance;	// 정적 변수
	
	private Singleton() {}	// 생성자
	
	public static Singleton getInstance() {		// 정적 메소드
		if(uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	}

}
```



아직 인스턴스가 만들어지지 않았다면 private으로 선언된 생성자를 이용해서 객체를 만든 다음 unique Instance에 객체를 대입한다. 이렇게 하면 인스턴스가 필요한 상황이 닥치기 전에는 아예 인스턴스를 생성하지 않는다. 이런 방법을 **게으른 인스턴스(Lazy Instantiation)** 이라 부른다.





## 사용 이유

- 고정된 메모리 영역을 얻으면서 하나의 인스턴스를 사용하기 떄문에 메모리 낭비를 받지할 수 있다.
- 싱글톤 객체는 **전역 인스턴스**이기 때문에 다른 클래스들의 인스턴스들이 데이터를 공유하기 쉽다.
- DBCP(DataBase Connection Pool)처럼 공통된 객체를 여러개 생성해서 사용해야 하는 경우에 많이 사용한다.
- 스레드풀, 대화상자, 로그 기록용 객체, 프린터 디바이스 드라이버 등 사용





## 문제점

- **다중 스레드**에서 동기화 처리를 하지 않으면 **인스턴스가 1개 이상 생성**되는 경우가 발생할 수 있다.

![singleton](https://user-images.githubusercontent.com/23491962/98838289-65adb580-2487-11eb-820f-629a48dfb6a7.JPG)





## 해결책

1. getInstance() 를 동기화 =>  **synchronized** 키워드

```java
Public class Singleton {
	private static Singleton uniqueInstance;	// 정적 변수
	
	private Singleton() {}	// 생성자
	
	public static synchronized Singleton getInstance() {		// 정적 메소드
		if(uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	}

}
```

- synchronized 키워드를 추가하면 한 스레드가 메소드 사용을 끝내기 전까지 다른 스레드는 기다려야 한다. 두 스레드가 이 메소드를 동시에 실행하는 일은 일어나지 않는다.
- 하지만 동기화를 사용한다면 성능이 100배 정도 저하된다.



2. 인스턴스를 필요할 때 생성하지 않고, **처음부터 생성**

```java
Public class Singleton {
	private static Singleton uniqueInstance = new Singleton();	// 정적 변수
	
	private Singleton() {}	// 생성자
	
	public static Singleton getInstance() {		// 정적 메소드
		return uniqueInstance;
	}
}
```

- 클래스가 로딩될 때 JVM에서 Singleton 인스턴스를 생성한다.



3. **DCL(Double-Checking Locking)** 사용

```java
Public class Singleton {
	private volatile static Singleton uniqueInstance;	// 정적 변수
	
	private Singleton() {}	// 생성자
	
	public static Singleton getInstance() {		// 정적 메소드
		if(uniqueInstance == null) {
			synchronized (singleton.class) {
				if(uniqueInstance == null) {
					uniqueInstance = new Singleton();
				}
			}
		}
		return uniqueInstance;
	}

}
```

- DCL을 사용하면 일단 인스턴스가 생성되어 있는지 확인한 다음, 생성되어 있지 않았을 때만 동기화할 수 있다.
- 처음에만 동기화를 하고 나중에는 동기화를 하지 않아도 된다.
- 1번 해결책에서 나타나는 속도문제를 줄일 수 있다.
