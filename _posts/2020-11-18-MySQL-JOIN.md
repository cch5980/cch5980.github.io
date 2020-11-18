---
title: "MySQL : JOIN"
category: MySQL, Database, Query
---



## 서론

- 최근에 실시간 주식 데이터를 가지고 분산 디비화 프로젝트를 진행하고 있다. 스프린트1이 곧 마무리 되어지는 단계에서 남은 기능 중 하나인 분봉 차트 그리는 작업에서 약간 시간이 지연되고 있다. 그 이유는 초 단위로 입력된 데이터들을 분단위로 그룹화처리를 해주고 시가, 고가, 저가, 종가 등의 원하는 데이터들로 얻어와야 되는데 아직 쿼리에 대한 기본기가 탄탄하지 않아서 난항을 겪고 있다. 그래서 이번 포스팅은 Join에 대해서 공부해보도록 하겠다.



## JOIN

- INNER 조인
- LEFT OUTER, RIGHT OUTER, FULL OUTER 조인
- 카티전 조인(CROSS 조인)
- 셀프 조인



## 실습

```sqlite
CREATE TABLE singer ( id INT, name VARCHAR(32), debut DATE, hit_song_id INT );

CREATE TABLE song ( id INT, title VARCHAR(32), lyrics VARCHAR(32) );



INSERT INTO song (id, title, lyrics) VALUES (101, '다이너마이트', '라라라라라라라라라라'); 
INSERT INTO song (id, title, lyrics) VALUES (102, 'Pyscho', '이해가 안된대 서로 좋아 죽는 바보'); 
INSERT INTO song (id, title, lyrics) VALUES (103, 'FLEX', '그냥 노창처럼 사라져'); 
INSERT INTO song (id, title, lyrics) VALUES (104, '빈지노', '빈지노의 돈 빈지노의 차 빈지노의 옷'); 
INSERT INTO song (id, title, lyrics) VALUES (105, '검은 행복', '내 파피는 흑인 미군 후하'); 
INSERT INTO song (id, title, lyrics) VALUES (106, '기대해', '기대해'); 
INSERT INTO song (id, title, lyrics) VALUES (107, '여수밤바다', '여수 밤바다 그 조명에 담긴'); 
INSERT INTO song (id, title, lyrics) VALUES (108, '암욜맨', '암욜맨 그대여'); 
INSERT INTO song (id, title, lyrics) VALUES (109, '사랑안해', '이제 다시 사랑안해'); 
INSERT INTO song (id, title, lyrics) VALUES (110, '남자기 때문에', '빨래비누로 머리를 감아도 개운해'); 
INSERT INTO song (id, title, lyrics) VALUES (111, '울어', '울어 울어 울었어'); 
INSERT INTO song (id, title, lyrics) VALUES (112, '나혼자', '나 혼자 밥을 먹고 나 혼자 영화 보고'); 
INSERT INTO song (id, title, lyrics) VALUES (113, '편의점', '저기요 계산 아직 안하셨는데요'); 
INSERT INTO song (id, title, lyrics) VALUES (114, '너희가 힙합을 아느냐', '유앤유 더블유 왜씨케이 후'); 
INSERT INTO song (id, title, lyrics) VALUES (115, '위아래', '위 아래 위위 아래'); 
INSERT INTO song (id, title, lyrics) VALUES (116, '잠시만 안녕' , '잠시만 울자 널 위해 안녕');



INSERT INTO singer (id, name, debut, hit_song_id) VALUES (1, 'BTS', '2020-01-01', 101); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (2, '레드벨벳', '2020-02-01', 102); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (3, '스윙스', '2020-03-01', 103); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (4, '블랙넛', '2020-05-01', 104); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (5, '윤미래', '2020-05-01', 105); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (6, '장범준', '2020-06-01', 107); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (7, '백지영', '2020-07-01', 109); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (8, '타이거JK', '2020-08-01', 110); 
INSERT INTO singer (id, name, debut, hit_song_id) VALUES (9, 'VOS', '2020-09-01', 111); 
INSERT INTO singer (id, name, debut) VALUES (10, '마룬5', '2020-10-01'); 
INSERT INTO singer (id, name, debut) VALUES (11, '존레전드', '2020-11-01');
```

![join1](C:\Users\cch\Desktop\join1.JPG)

![join2](C:\Users\cch\Desktop\join2.JPG)



**1) INNER 조인**

-  두개의 집합(A, B)의 교집합 이라고 생각하면 된다.

```sql
SELECT A.id, A.name, B.title 
FROM singer AS A 
JOIN song AS B 
ON B.id = A.hit_song_id;
```

![join3](C:\Users\cch\Desktop\join3.JPG)



**2) LEFT OUTER, RIGHT OUTER, FULL OUTER 조인**

- 두 테이블에서 지정된 쪽인 LEFT 또는 RIGHT 쪽의 모든 결과를 보여준후 반대쪽에 매칭되는 값이 없어도 보여주는 JOIN이다.
- JOIN 이전에 나오는 테이블이 왼쪽(LEFT)테이블이 되고, JOIN 이후에 나오는 테이블이 오른쪽(RIGHT)테이블이 된다.

```sql
SELECT A.id, A.name, B.title 
FROM singer AS A LEFT OUTER 
JOIN song AS B 
ON B.id = A.hit_song_id; 


SELECT B.id, B.title, A.name 
FROM singer AS A RIGHT OUTER 
JOIN song AS B 
ON B.id = A.hit_song_id;


SELECT A.id, A.name, B.id, B.title 
FROM singer AS A FULL OUTER 
JOIN song AS B 
ON B.id = A.hit_song_id; 
```

![join5](C:\Users\cch\Desktop\join5.JPG![join4](C:\Users\cch\Desktop\join4.JPG)

현재 SQLite에서는 RIGHT OUTER 조인과 FULL OUTER 조인은 지원되고 있지 않아 다음과 같이 오류가 발생한다.

![join5](C:\Users\cch\Desktop\join5.JPG)



**3) 카티전 조인(CROSS 조인)**

- 집합 곱의 개념이다.
- A = {a,b,c,d}, B = {1,2,3} 일 때 A CROSS JOIN B는 (a,1), (a,2), (a,3),  (b,1), (b,2), (b,3),  (c,1), (c,2), (c,3),  (d,1), (d,2), (d,3) 이다.

```sql
SELECT B.id, B.title, A.name 
FROM singer AS A CROSS 
JOIN song AS B;
```

![join6](C:\Users\cch\Desktop\join6.JPG)





## 정리

- 오늘은 INNER 조인에 대해서만 공부하려고 했는데 테이블을 직접 만들고 예제 데이터도 일일히 만들다보니 CROSS 조인까지 살펴보았다. 조인 관련해서 어렵게 생각할 것 없이 그림으로 교집합 관계를 생각하면 되겠다.
- 그리고 SQLite 에서 지원이 안되는 문법이 생각보다 많다.

