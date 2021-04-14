# SQL(Structured Query Language)

## DBMS(DataBase Management System)

* Relational DB
* Hierarchical DB
* Flat Files DB
* Objects DB

<br>

### 데이터 베이스

> C.R.U.D(Create, Read, Update, Delete), [ ]는 생략가능
>
> DML (Data Manipulation Language) : commit, rollback 가능 - Create, Update, Delete

* Create : insert into 테이블명[(컬럼명리스트,...)] values (데이터값리스트,...)

* Update : update 테이블명 set 컬럼명 = 컬럼값, ...[where 조건식]

* Delete : delete [from] 테이블명 [where 조건식]

* **Read** 

  > Select 필수, 나머지는 생략 가능
  >
  > 문자열 데이터 값 : 단일 인용부호만 사용 가능

  * Select 컬럼명리스트
  * from 테이블명 
  * where 조건식 (행에 대한 조건)
  * group by 그룹핑 기준이 되는 컬럼
  * having 그룹 조건 (그룹에 대한 조건)
  * order by 정렬 기준이 되는 컬림 [desc]
  * AS : 컬럼 별칭 구문
  * Distinct : 컬럼에 포함된 중복 값을 한번만 표시

<br>

#### Select

* select * from t where empno = 100
* select * from t where empno != 100
* select * from t where ename = 'King'
* select * from t where ename = 'King' or ename = 'Kang' or ename = 'Kong'
* select * from t where ename in ('King', 'Kang', 'Kong')
* select * from t where ename like 'K%'
  * % : 0개 이상의 임의의 문자 ('K%', '%K', '%K%')
  * _ : 임의의 한 문자 

<br>

<br>

#### Create





#### 현재시간 내보내기

* Maria DB : select now();
* Oracle DB : select sysdate from dual; 

<br>

### 연산자

#### 연결 연산자

* 연결 연산자 ```||```를 사용하여 여러 컬럼을 연결하거나, 컬럼과 문자열을 연결할 수 있다.

```sql
-- 컬럼과 컬럼을 연결한 경우
SELECT empno||ename||sal
FROM EMPLOYEE;

-- 컬럼과 리터럴을 연결한 경우
SELECT empno||'의 월급은'||sal||'원 입니다.'
FROM EMPLOYEE;
```

<br>

#### 논리 연산자

* 여러 개의 제한 조건 결과를 하나의 논리 결과(TRUE/ FALSE/ NULL)로 만들어준다.

| Operator | 의미                                                      |
| -------- | --------------------------------------------------------- |
| AND      | 여러 조건이 동시에 TRUE일 경우에만 TRUE 값 반환           |
| OR       | 여러 조건들 중에 어느 하나의 조건만 TRUE이면 TRUE 값 반환 |
| NOT      | 조건에 대한 반대 값으로 반환 (NULL 제외)                  |

<br>

#### 비교 연산자

* 표현식 사이의 관계를 비교하기 위해 사용
* 비교 결과는 논리 결과 중의 하나(TRUE/ FALSE/ NULL)가 된다.
* 비교하는 두 컬럼 값/표현식은 서로 동일한 데이터 타입이어야 한다.

| Operator              | 의미                                |
| --------------------- | ----------------------------------- |
| =                     | 같다                                |
| > , <                 | 크다 / 작다                         |
| >= , <=               | 크거나 작다 / 작거나 같다           |
| <> , != , ^=          | 같지 않다                           |
| BETWEEN AND           | 특정 범위에 포함되는지 비교         |
| LIKE / NOT LIKE       | 문자 패턴을 비교                    |
| IS NULL / IS NOT NULL | NULL 여부 비교                      |
| IN                    | 비교 값 목록에 포함되는지 여부 비교 |

<br>

#### BETWEEN  AND

* 비교하려는 값이 지정한 범위에 포함되면 TRUE를 반환하는 연산자

```sql
SELECT ENAME, SAL
FROM EMPLOYEE
WHERE SAL BETWEEN 3500 AND 5500;

-- or

SELECT ENAME, SAL
FROM EMPLOYEE
WHERE SAL >= 3500
AND   SAL <= 5500;
```

<br>

#### LIKE

* 비교하려는 값이 지정한 특정 패턴을 만족시키면 TRUE를 반환하는 연산자
* 패턴 지정을 위해 와일드 카드 사용

| 기호           | 의미                                                  |
| -------------- | ----------------------------------------------------- |
| % (Percentage) | % 부분에는 임의 문자열(0개 이상의 문자)이 있다는 의미 |
| _ (Underscore) | _ 부분에는 문자 1개만 있다는 의미                     |

```sql
-- '김'씨 성을 가진 직원 이름과 급여 조회

SELECT ENAME, SAL
FROM EMPLOYEE
WHERE ENAME LIKE '김%'

-- 9000번대 4자리 국번의 전화번호를 사용하는 직원 전화번호 조회
SELECT ENAME, PHONE
FROM EMPLOYEE
WHERE PHONE LIKE '___9________';
```

<br>

* Escape Option : 와일드 카드 ('%', '_') 자체를 데이터로 처리해야 하는 경우에 사용

```sql
-- 전체 데이터가 모두 조회됨 

SELECT ENAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE '____%';

-- email id 중 '_' 앞 자리가 3자리인 직원 조회
-- ex) jih_abc@naver.com

SELECT ENAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE '___\_%' ESCAPE '\';

-- ESCAPE OPTION에 사용하는 문자는 임의 지정 가능
SELECT ENAME, EMAIL
FROM EMPLOYEE
WHERE EMAIL LIKE '___#_%' ESCAPE '#';
```

<br>

#### NOT LIKE

```sql
-- '김'씨 성이 아닌 직원 이름과 급여 조회

SELECT ENAME, SAL
FROM EMPLOYEE
WHERE ENAME NOT LIKE '김%';

-- or

SELECT ENAME, SAL
FROM EMPLOYEE
WHERE NOT ENAME LIKE '김%';
```

<br>

#### IS NULL / IS NOT NULL

* NULL 여부를 비교하는 연산자

```sql
-- 관리자도 없고 부서 배치도 받지 않은 직원 이름 조회

SELECT ENAME, MGR, DEPT_ID
FROM EMPLOYEE
WHERE MGR IS NULL
AND DEPT_ID IS NULL;

-- 부서 배치를 받지 않았음에도 보너스를 지급받은 직원 이름 조회

SELECT ENAME, DEPT_ID, BOUNUS
FROM EMPLOYEE
WHERE DEPT_ID IS NULL
AND BONUS IS NOT NULL;
```

<br>

#### IN 연산자

* 비교하려는 값 목록에 일치하는 값이 있으면 TRUE를 변환하는 연산자

```sql
-- 60번 부서나 90번 부서원들의 이름, 부서 코드, 급여 조회

SELECT ENAME, DEPT_ID, SAL
FROM EMPLOYEE
WHERE DEPT_ID IN ('60', '90');

-- or

SELECT ENAME, DEPT_ID, SAL
FROM EMPLOYEE
WHERE DEPT_ID = '60'
OR    DEPT_ID = '90';
```

<br>

#### 연산자 우선 순위

* 여러 연산자를 함께 사용할 때 우선 순위

| 순위 | 연산자                        |
| ---- | ----------------------------- |
| 1    | 산술 연산자                   |
| 2    | 연결 연산자                   |
| 3    | 비교 연산자                   |
| 4    | IS (NOT) NULL, LIKE, (NOT) IN |
| 5    | (NOT) BETWEEN-AND             |
| 6    | 논리 연산자 - NOT             |
| 7    | 논리 연산자 - AND             |
| 8    | 논리 연산자 - OR              |

```sql
-- 논리연산자 AND가 먼저 처리되어 '급여 3000이상인 90번 부서원' or '20번 부서원' 조회

SELECT ENAME, SAL, DEPT_ID
FROM EMPLOYEE
WHERE DEPT_ID = '20'
OR    DEPT_ID = '90'
AND   SAL > 3000;

-- 20번 또는 90번 부서원 중 급여를 3000원 보다 많이 받는 직원 이름, 급여, 부서코드 조회
-- 논리연산자 OR가 먼저 처리되도록 ( )를 이용

SELECT ENAME, SAL, DEPT_ID
FROM EMPLOYEE
WHERE (DEPT_ID = '20'
OR    DEPT_ID = '90')
AND   SAL > 3000;
```



