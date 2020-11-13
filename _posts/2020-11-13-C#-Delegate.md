---
title: "C# Delegate"
category: C#, Programming Language
---



## Delegate

- C#에서 메소드를 가리킬 수 있는 타입
- 메소드를 다른 메소드로 전달할 수 있도록 한다.
- 메소드를 직접 호출하는 것 대신에 delegate로 그 메소드를 호출할 수 있다.



## 1. delegate 만들기

delegate가 어떤 메소드를 가리키기 때문에 그 메소드와 동일한 매개변수와 리턴타입으로 선언해야 한다.

```c#
delegate void typeA(void);		// void funcA(void)
delegate int typeB(int, int);	// int funcB(int, int)
delegate string typeC(double);	// string funcC(double)
delegate float typeD(int);		//float funcD(int)
```



## 2. delegate 사용

선언한 델리게이트 타입으로 델리게이트 변수를 생성하고, 생성한 델리게이트 변수에 해당 메소드를 참조시킨다.

```c#
// delegate 변수 생성
typeA delegate1;
typeB delegate2;
typeC delegate3;
typeD delegate4;

// 메소드 참조
delegate1 = new typeA( funcA );
delegate2 = new typeB( funcB );
delegate3 = new typeC( funcC );
delegate4 = new typeD( funcD );
```



## 3. 간단한 사용 예제

![delegate1](C:\Users\cch\Desktop\delegate1.JPG)

![delegate2](C:\Users\cch\Desktop\delegate2.JPG)



## 4. 사용 이유(Why)

- **콜백 메소드**에 활용된다.
- 콜백 : A라는 메소드를 호출할 때, B라는 메소드를 매개변수로 넘겨주고 A메소드에서 B메소드를 호출하는 것
- 아래 예제를 보면 계산기 함수를 호출할 때마다 원하는 **연산(메소드)**를 넘겨주어 계산기가 해당 기능을 하고 있다. 
- ![delegate3](C:\Users\cch\Desktop\delegate3.JPG)
- ![delegate4](C:\Users\cch\Desktop\delegate4.JPG)





## 5. 델리게이트 체인

- 델리게이트는 하나의 메소드 뿐만 아니라 여러개의 메소드를 참조할 수 있다.
- '+' 연산자를 이용하여 메소드를 추가해준다.

```
MyDelegate del;
del = new MyDelegate( func1 );
del += func2;
del += func3
```

![delegate5](C:\Users\cch\Desktop\delegate5.JPG)

![delegate6](C:\Users\cch\Desktop\delegate6.JPG)