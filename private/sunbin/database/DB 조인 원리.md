# DB JOIN/원리

### JOIN이란?

두 개 이상의 table에 있는 데이터를 한 번에 조회하는 것

두 table에서 일치하는 tuple값을 가진 행들을 가져온다

### INNER JOIN

두 table에서 join condition을 만족하는 tuple들로 result table을 만드는 join

두 table에서 ON절의 조건을 만족하는 result table을 만드는 join

ON절에는 두 table에 대한 관계를 정의하는 조건이 들어가야 한다

```sql
FROM table1 [inner] JOIN table2 ON join_condition
```

**LEFT JOIN**

첫 table에 해당하는 행들을 result table에서 보존 해야 할 때

```sql
SELECT *
FROM FinishLine_DB.graduation_mydonelecture AS m 
	LEFT JOIN FinishLine_DB.user_user AS a
ON m.user_id = a.student_id;
```

해당 LEFT JOIN문을 조회하면 왜 두 table의 속성이 전부 조회가 되는가?

**LEFT JOIN의 역할은 행을 보존하기 위함이지 속성을 보여주는 역할이 아니다**

null값인 점은 무시하지 않는다.

속성은 SELECT에서 결정한다.

그렇다면 굳이 JOIN이 아니라 LEFT JOIN을 쓸 이유가 무엇인가?

어차피 속성을 SELECT에서 결정 해야 하는데?

```sql
-- 예) 지도교수 지정 여부를 확인하기 위해 1학년 학생의 이름을 전부 출력하라

SELECT student.NAME, professor.NAME 
FROM student 
	LEFT OUTER JOIN professor
ON student.PID = professor.ID 
WHERE grade = 1
```

해당 join문을 통해 join을 했음에도 지도 교수가 지정되지 않은 학생들 또한 result table에 존재할 수 있다

**이를 통해 LEFT JOIN은 속성들을 보존하는 것이 아니라 행을 보존하기 위한 JOIN문으로 해석할 수 있다.**

**RIGHT JOIN**

두 번째 table에 해당하는 행들을 result table에서 보존 해야 할 때

설명은 LEFT JOIN과 동일하다

### FULL OUTER JOIN

왼쪽 table의 모든 행과 오른쪽 table의 모든 행을 전부 조회하는 것

없는 값은 null 처리

전체 행을 전부 출력함에도 두 table간 일치하는 속성은 존재 해야 한다.

### JOIN 알고리즘

Optimizer가 join이 포함된 쿼리문을 분석하여 실행 시간을 기반으로 아래 알고리즘 중 하나를 선택하여 최적의 실행 계획을 수립한다.

1. **Nested Loops Join (중첩 루프 조인)**
2. **Sort Merge Join (정렬 병합 조인)**
3. **Hash Join (해시 조인)**

**Nested Loops Join (중첩 루프 조인)**

두 table에서 순차적으로 행을 조회하여 원하는 결과를 조합하는 알고리즘

장점

- 단순하다
- 데이터셋이 작을수록 빠르게 수행된다

단점

- 대규모 데이터셋에서 비효율적이다
- 인덱스가 없다면 full scan 가능성이 존재한다.

큰 구조는 이중 for문과 동일하다

1. 외부 루프 : 첫 번째 table의 각 행을 순차적으로 읽는다
2. 내부 루프 : 두 번째 table의 각 행을 읽어 외부 루프에서 선택된 행과 비교한다.
3. join 조건 검사 : join 조건을 만족하는 행을 찾으면 결과에 추가한다.

```sql
 for(i=0; i<driving table.length; i++) {
    for(j=0; j<driven table.length; j++) {
       // Search
    } 
}
```

![스크린샷 2026-01-19 012006.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-19_012006.png)

**DRIVING TABLE/DRIVEN TABLE (선행 / 후행 테이블)**

선행 테이블, Join시 먼저 Access되어 Access Path를 주도하는 table을 driving table이라 한다

후행 테이블, join시  나중에 Access되는 table을 driven table이라 한다.

index의 존재 여부, 통계 정보(조건에 따라 얼마나 줄어드는지)에 따라 선행 / 후행 테이블을 결정하는데 영향을 준다.

어느 table이 선행 테이블이 되냐에 따라 속도의 차이가 크게 날 수 있다.

선/후행 테이블을 결정하는 요소는 무엇인가?

**비용 기반 옵티마이저(Cost-Based Optimizer, CBO)**

쿼리를 수행하는데 소요되는 시간을 계산해 실행 계획을 생성한다.

옵티마이저의 고유 기능으로 개발자가 직접 수정/편집할 수 없다

다만 개발자는 INDEX튜닝이나 HINT로 실행 계획을 권고 할 수 있다

**Sort Merge Join (정렬 병합 조인)**

두 개의 table을 join 컬럼을 기준으로 오름차순 정렬한 후, 두 개의 테이블에서 정렬된 join 대상 키를 스캔하면서 조인 결과를 생성하는 알고리즘

장점

- NL Join보다 대용량 데이터에 효과적이다.

단점

- 정렬할 데이터가 많을 경우 정렬 비용에 의해 성능 저하 발생

작동 방식은 다음과 같다

1. 선행 테이블에서 정렬 작업 수행
2. 후행 테이블에서 정렬 작업 수행
3. 정렬된 결과를 이용하여 조인을 수행

![스크린샷 2026-01-19 020135.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-19_020135.png)

**Hash Join (해시 조인)**

두 table중 하나를 해시 table로 선정하여 조인될 테이블의 조인 키 값을 해시 알고리즘으로 비교하여 매치되는 결과값을 얻는 방식

1. 선행 테이블에서 주어진 조건을 만족하는 행을 찾음
2. 선행 테이블의 조인 키를 기준으로 해시 테이블 생성
3. 후행 테이블에서 조건에 만족하는 행을 찾음
4. 만족하는 행을 해시 테이블로 직접 찾아 저장함

![스크린샷 2026-01-19 021436.png](%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2026-01-19_021436.png)

### 실행 순서

옵티마이저는 기본적으로 쿼리문을 받고 SQL쿼리문 순서에 구애받지 않고 

어떤 알고리즘을 정할 지 우선 선택한다.

**NL JOIN**

조인의 기본은 NL조인이다.

인덱스를 이용한 조인 방식이다

Driving table이 작아야 한다

```sql
SELECT e.사원명, c.고객명, c.전화번호
FROM 사원 e, 고객 c
WHERE e.입사일자 >= '19960101'
AND c.관리사원번호 = e.사원번호
```

```sql

0 SELECT STATEMENT
1  NESTED LOOPS
2   TABLE ACCESS BY INDEX ROWID  #사원
3    INDEX RANGE SCAN            #사원_X1
4   TABLE ACCESS BY INDEX ROWID  #고객
5    INDEX RANGE SCAN            #고객_X1
```

1. E table에서 입사일자를 기준으로 range scan(인덱스에 따른 실제 table 조회 방식)
2. E table과 C table의 join 조건으로 table 조회
3. 각 사원명, 고객명, 전화번호를 조회하여 결과 집합에 반환
4. 다 찾을 때까지 1~3 반복

**NL JOIN 선택 이유**

E table에는 인덱스가 존재했기 때문에

where절의 범위 설정으로 driving table의 범위가 적었다

**ROWID란?**

인덱스에서 받은 값의 실제 table 내 물리적 위치

**Sort Merge JOIN**

조인 컬럼에 인덱스가 없거나, 대량 데이터일 때

Hash join을 사용할 수 없는 상황일 때

```sql
SELECT /*+ ordered use_merge(c) */
    e.사원번호, e.사원명, e.입사일자, c.고객명, c.전화번호
FROM 사원 e, 고객 c
WHERE c.관리사원번호 = e.사원번호
AND e.입사일자 >= '19960101'
AND e.부서코드 = 'Z123'
AND c.최종주문금액 >= 20000
```

```sql

  [E 테이블]        [C 테이블]
   입사일자 필터      주문금액 필터
       │                  │
   사원번호 정렬       관리사원번호 정렬
       │                  │
       └─── Merge Join ───┘
               │
      Projection (사원명, 고객명, 전화번호)
               │
           결과 반환
```

1. E table에서 입사일자, 부서코드를 기준으로 필터링한다
2. E table을 정렬한다
3. C table을 최종주문금액을 기준으로 필터링한다
4. C table을 정렬한다
5. Join 조건인 관리사원번호, 사원번호를 통해 출력 컬럼을 선택한다
6. 다 찾을 때까지 5를 반복한다

**HASH JOIN**

대량 데이터를 join할 때 sort merge join에 비해 정렬하는 부담이 들지 않는다

Hash join은 **build, probe** 단계로 나뉘어져 있다

```sql
SELECT /*+ ordered use_hash(c) */
    e.사원번호, e.사원명, e.입사일자, c.고객명, c.전화번호
FROM 사원 e, 고객 c
WHERE c.관리사원번호 = e.사원번호
AND e.입사일자 >= '19960101'
AND e.부서코드 = 'Z123'
AND c.최종주문금액 >= 2000
```

1. **Build 단계** (작은 쪽 테이블 선택):
    - C table에서 최종주문금액으로 필터링
    - 관리사원번호를 해시 함수에 넣어 hash table 생성
2. **Probe 단계** (큰 쪽 테이블 선택):
    - E table의 입사일자, 부서코드로 필터링
    - E table의 사원번호에 맞는 조건을 hash table 내부에서 탐색
    - join 조건 만족시 결과 반환

https://velog.io/@eunhye_/SQL-%EC%A1%B0%EC%9D%B8Join-%EC%88%98%ED%96%89-%EC%9B%90%EB%A6%AC

https://devuna.tistory.com/36

https://minnseong.tistory.com/48