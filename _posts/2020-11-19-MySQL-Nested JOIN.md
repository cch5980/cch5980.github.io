---

title: "MySQL : Nested JOIN"
category: MySQL, Database, Query
---



## 서론

- 최근 프로젝트로 인해 복잡한 쿼리를 짜다보니 subquery 가 2개,3개 생기는 경우가 발생하였고 그에 따른 성능에 관해서 고민하게 되었다. 그래서 일단 오늘은 Nested Join 방식에 대해 알아보도록 하겠다.



## JOIN

- 두 릴레이션으로부터 관련된 투플을 결합하여 하나의 투플로 만드는 데이터 연결 방법
- 다른 조인 방법을 사용할 지라도 결과는 동일할 수 있으나, 두 집합의 데이터의 상황에 따라 조인 방향, 방법에 따른 수행 속도가 다를 수 있다.



## Nested Loop Join

- 2개 이상의 테이블에서 하나의 집합을 기준으로 순차적으로 상대방 로우를 결합하여 결과를 조합하는 방식

- 먼저 선행 테이블의 처리 범위를 하나씩 액세스하면서 추출된 값으로 테이블을 조인한다.

- 쉽게 생각하면 흔히 보는 이중 for문이라고 보면 된다.

  - ```java
    for(i=0; i<100; i++) { -- outer loop 
    	for(j=0; j<100; j++) { -- inner loop 
       		// 처리... 
    	} 
    }
    ```

- ![nested join1](C:\Users\cch\Desktop\nested join1.jpg)





- 특징
  - 좁은 범위에서 유리한 성능을 보인다.
  - 순차적으로 처리고, Random Access 위주로 처리한다.
  - 후행 테이블(Driven)에는 조인을 위한 인덱스 생성이 필요하다.
  - 실행 속도 = 선행 테이블 사이즈 * 후행 테이블 접근 횟수

- 단점

  - 데이터를 Random Access 하기 때문에 결과 집합이 많으면 성능이 느리다.
  - Join Index가 없거나, 조인 집합을 구성하는 검색 조건이 조인 범위를 줄여주지 못할 경우에 비효율적이다.
  - 테이블 중 Row수가 적은 쪽을 후행(Driven) 테이블로 설정해야 한다.

- 예제

  - ```sql
    SELECT /*+ ordered use_nl(e)*/
    FROM dept d, emp e 
    WHERE d.deptno = e.deptno;
    ```

    





## Sort Merge Join

- 조인의 대상 범위가 넓을 경우 발생하는 Random Access를 줄이기 위한 경우에 사용한다.
- 연결 고리에 마땅한 인덱스가 존재하지 않을 경우에 사용되기도 한다.
- 양쪽 테이블의 처리 범위를 각자 Access하여 정렬한 결과를 차례로 scan하면서 연결고리의 조건으로 Merge하는 방식이다.



- ![nested join2](C:\Users\cch\Desktop\nested join2.jpg)





- 특징
  - 연결을 위해 Random Access 하지 않고 스캔을 하면서 수행한다.
  - 선행 집합의 개념이 없다.
  - 정렬을 위한 영역에 따라 효율에 큰 차이가 발생한다.
  - 조인 연산자가 **'='** 가 아닌 경우 Nested Loop Join 보다 유리한 경우가 많다.
- 단점
  - 두 결과의 집합 크기의 차이가 많이 나는 경우에는 비효율적이다.
  - Sorting 에 해당하는 대상이 Join Key 뿐만 아니라 Select List 도 포함되므로 불필요한 Select 항목을 제거할 필요가 있다.

- 예제

  - ```sql
    SELECT /*+ ordered full(d) use_merge(e)*/
    FROM   dept d, emp e
    WHERE  d.deptno = e.deptno;
    ```

    