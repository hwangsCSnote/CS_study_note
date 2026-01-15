# DB 인덱스

### 인덱스 정의

추가적인 쓰기 작업과 추가 공간 활용을 통해 원하는 값을 **빠르게** 찾기 위한 자료구조이다.

예시로 책에서 원하는 파트를 찾기 위해 책을 전부 확인하는 것보다  저자가 정리해 놓은 색인을 보는 것과 같다.

- 장점
    1. table 조회 속도가 향상된다.
    2. 시스템 부하를 줄일 수 있다.
- 단점
    1. 인덱스를 관리하기 위해 DB의 약 10%에 해당하는 저장 공간이 필요하다.
    2. 인덱스를 관리하기 위해 추가 작업이 필요하다.

### 인덱스 필요성

```sql

select * from user where user_name = '황선빈';

#텍스트일 경우 탐색 방법
***#utf-8 설정에 따라 정렬 방법이 달라짐
```

해당 조건을 탐색한다고 했을 때 쿼리문은 user table안에 존재하는 user_name을 전부 탐색함

시간 복잡도는 O(n)

그러나 B-tree 기반의 인덱스 계산을 하게 되면 O(logn)으로 줄일 수 있음

```sql
select * from user where user_name = '황선빈';
delete from mydonelecture where user_id = '20191501';
update from alllecturedata where major = '의예';
select * from user join mydonelecture where user_id = '20191501';
```

다음과 같은 모든 쿼리문에서 where문에 해당하는 ‘조건’을 빨리 찾는 것이 인덱스를 사용하는 이유이다.

### 인덱스 만드는 법

**기본 문법**

기존에 존재하는 player table에 인덱스를 만든다고 하면 크게 두 가지 종류의 인덱스를 만들 수 있다.

```sql
#기존 테이블
player table (
	id
	name
	team_id
	backnumber
)
```

```sql
#기존 테이블에서 인덱스 생성

#중복 허용하는 인덱스
CREATE INDEX name_idx ON player (name);

#중복을 허용하지 않는 인덱스
CREATE UNIQUE INDEX team_id_backnumber_idx ON player (team_id, backnumber);
```

처음 테이블을 생성할 때 인덱스를 설정할 수도 있다

```sql
#테이블 생성시 인덱스 설정

CREATE TABLE player (
	id int primary key
	name varchar not null,
	team_id int,
	backnumber int,
	INDEX name_idx (name),
	UNIQUE INDEX team_id_backnumber_idx (team_id, backnumber)
)
```

**INDEX 모니터링**

index는 직접 설정하지 않아도 기본 설정으로 존재한다.

기존 테이블에 index가 있는 지 확인하는 방법도 존재한다.

```sql
SHOW INDEX FROM player;
```

### B-TREE 기반 동작 방법

**B-TREE 기본 구조(이진 트리)**

![스크린샷 2026-01-15 102106.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-15_102106.png)

*이진 탐색 트리* 기본 설명은 다음과 같다

- 각 노드보다 크기가 작은 것은 왼쪽으로 배치된다.
- 각 노드보다 크기가 큰 것은 오른쪽으로 배치된다.

이를 DB INDEX에서 값을 찾는 방식에서 찾아볼 수 있다.

**INDEX가 하나의 attribute만을 탐색할 경우**

![스크린샷 2026-01-15 105048.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-15_105048.png)

a에 대한 index를 생성했다면 DB는 기본적으로 a에 대해 정렬된 테이블을 가지고 있게 된다.

또한 각 행에 대한 포인터를 가지고 있어 찾았을 때 바로 실제 table의 행으로 넘어갈 수 있게 해준다.

정렬된 a를 통해 B-tree를 활용하여 값을 효율적으로 탐색한다.

```sql
where index(a) = 9;
```

1. 정렬된 테이블의 중앙값(a=5)을 우선 탐색한다
    1. index a = 5 이므로 5 아래 값은 탐색에서 제외한다
2. 좁혀진 구간의 중앙값(a=7)을 우선 탐색한다.
    1. index a = 7 이므로 7 아래 값은 탐색에서 제외한다.
3. 좁혀진 구간의 중앙값인 index a = 9 탐색한다.
    1. index a = 9 이므로 값이 일치한다.
    2. 포인터로 연결된 값을 찾아낸다.

**INDEX가 여러 개의 attribute를 탐색할 경우**

```sql
where index(a) = 9 and index(b) = 95; 
```

이처럼 and연산자로 여러 attribute의 조건을 만족시키는 행을 찾아야할 경우 인덱스 table은 다음과 같이 정렬된다.

![스크린샷 2026-01-15 110034.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-15_110034.png)

해당 table을 통해 조건을 탐색하는 순서는 다음과 같다.

1. 해당 table의 중앙값인 a = 5를 우선 탐색한다
    1. index a = 9 이므로 index = 5 아래의 값은 탐색에서 제외한다
2. 좁혀진 구간의 중앙값(a=7,  b=95)을 우선 탐색한다
    1. 일치하므로 포인터를 통해 해당하는 행을 가져온다

해당 table는 index(b)를 찾을 때 사용할 수 없는 table인 점을 명시 해야 한다.

table에 index(b)는 index(a)에 의해 정렬된 값들로써 오름 차순 되어있지 않다.

때문에 해당 table로 index(b)를 찾게 되면 index를 쓰지 않고 b = 95를 찾는 것보다 더 오래 탐색할 가능성이 있다.

다만, a에 대한 인덱스로써는 사용 가능하다.

### INDEX 최적화 방법

**Hash index**

해시 함수를 사용하여 데이터를 해시 테이블에 저장하는 인덱스

정확한 검색에 최적화 되어 있으며, 등호 연산(= ≠) 연산자의 경우에만 사용

범위 탐색(≤, ≥)에는 적합하지 않음 

**Covering index**

가져오려는 정보가 인덱스 테이블에 전부 포함이 되어있을 경우

```sql
select team_id, backnumber from player 
where team_id = 9 and backnumber = 95; 
```

![스크린샷 2026-01-15 113427.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-15_113427.png)

해당 코드를 통해 가져온다고 가정했을 때 player table에 team_id_back_number_idx의 인덱스 테이블을 활용하여 기존 player table을 탐색하지 않고 값을 찾을 수 있다.