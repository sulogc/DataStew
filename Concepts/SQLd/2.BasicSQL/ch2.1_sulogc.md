# Subject : SQL Basics and Implement


Contents
---

[Chapter 1. SQL 기본](#sql-기본)
- [Subject : SQL Basics and Implement](#subject--sql-basics-and-implement)
  - [Contents](#contents)
  - [SQL 기본](#sql-기본)
    - [관계형 데이터 베이스 개요](#관계형-데이터-베이스-개요)
    - [DDL](#ddl)
    - [DML](#dml)
    - [TCL](#tcl)
    - [WHERE](#where)
    - [함수](#함수)
    - [GROUP By and Having](#group-by-and-having)
    - [ORDER BY](#order-by)
    - [JOIN](#join)



<br>

---
<br>

## SQL 기본
<br>

### 관계형 데이터 베이스 개요

<br>

1-2 SQL 명령어 종류


- DML(Data Manipulation Language)
  
  조회, 검색, 데이터를 변형, 삽입, 삭제, 수정
  - SELECT
  - INSERT
  - UPDATE
  - DELETE

<br>

- DDL(Data Definition Language)

  데이터 구조를 정의. 테이블 생성, 삭제, 이름 바꾸기 등.
  - CREATE
  - ALTER
  - DROP
  - RENAME

<br>

- DCL(Data Definition Language)
  
  데이터베이스 접근, 객체들을 사용하도록 권한 부여, 회수.
  - GRANT
  - REVOKE

<br>

- TCL(Transaction Control language)
  
  - COMMIT
  - ROLLBACK

<br><br>

1-3 테이블

  테이블은 데이터를 저장하는 객체로서 관계형 데이터베이스의 기본 단위이다. Row, Column으로 구성, 겹치는 공간을 Field라고 함.

테이블을 분할하여 데이터의 불필요한 중복을 줄이는 것 -> 정규화

Primary Key: 각 행을 한 가지 의미로 특정할 수 있는 한 개 이상의 칼럼.

Foreign Key: 다른 테이블의 기본 키로 사용, 테이블의 관계를 연결하는 역할.

<br>

1-4 ERD(Entity Relationship Diagram)

테이블 간 서로의 상관 관계를 그림으로 도식화한 것. 구성 요소는 Entity, Relationship, Attribute, 이 세 가지로 현실 세계의 데이터는 이 3가지 구성 요소로 모두 표현이 가능함.
<br>
<br>

### DDL
<br>

- 데이터 유형
  - CHARACTER(s)/CHAR: 고정 길이 문자열 정보. 파라미터 s 만큼 최대 고정 길이를 가짐. 변수 값 길이가 s보다 작으면 나머지는 공간으로 채워짐. 공백 비교 불가능.
  - VERCHAR(s) : s만큼의 최대 길이를 갖지만 가변 길이로 조정이 됨. 길이를 예측 할 수 없는 경우 유용. 
  - NUMERIC : 정수, 실수 등 숫자 정보.
  - DATETIME : 날짜와 시각 정보

고정형인 CHAR가 더 빠르게 읽힌다. VERCHAR는 메모리 로스를 줄여줌.

- CREATE TABLE

  Candidate Key 중에 PK를 설정.
  (단일 키가 아닌 여러 키의 조합으로 구성가능.)
  
  - 테이블명은 중복불가.
  - 한 테이블 내 중복 칼럼 불가.(계층적 구조)
  - 칼럼은 다른 테이블까지 고려하여 일관성 있게 사용하는 것이 좋다.
  - 칼럼에 데이터 유형 지정되어야함.
  - A-Z, a-z, 0-9, _, $, # 문자만 허용됨.
  - 테이블 생성시 대/소 비교를 하지 않음.
  - DATETIME 데이터 유형은 크기 지정 안함.
  - 문자는 반드시 최대 길이 표시
  - 제약조건은 CONSTRAINT로 추가.
    - Table Level: PK, FK
    - Column Level: NOT NULL

- 제약 조건
  데이터 무결성을 유지하기 위한 데이터베이스는 보편적인 방법으로 테이블의 특정 칼럼에 설정. 
  - PRIMARY KEY : 행 데이터를 고유하게 식별하기 위해 기본키 정의. 
  테이블 당 하나의 기본키만 정의할 수 있다. 기본키 제약을 정의하면 자동으로 UNIQUE 인덱스 생성. 
  기본키 제약 = 고유키 제약 & NOT NULL제약

  - UNIQUE Key(고유키): 테이블에 저장된 행 데이터를 고유하게 식별하기 위한 고유키를 정의한다. NULL 값을 가진 행이 여러개 있을 수 있다.
  - FOREIGN key: 관계형 데이터베이스에서 테이블 간 관계를 정의하기 위해 기본키를 다른 테이블의 외래키로 복사하는 경우 생성.
  - NOT NULL, CHECK

NULL - 공백이나 숫자 0과는 전혀 다른 값. 데이터가 없을 때의 공집합과도 다름.
DEFAULT : 데이터 입력시 칼럼의 값이 지정되어 있지 않을 경우 기본값을 설정 가능. DEFAULT 정의 안할시 NULL이 입력된다.

- 테이블 확인.
DESCRIBE 테이블명;
exec sp_help 'dbo.테이블명' 

- SELECT를 통해 테이블 복사가 가능하다. 하지만, NOT NULL제약만 복사되고, 기본키, 고유키, 외래키, CHECK등 다른 제약조건은 없어진다. -> ALTER TABLE을 사용해야함.

- ALTER TABLE
  - ADD 칼럼 : 칼럼 및 제약 조건 변경, 추가
  - DROP 칼럼 : 삭제 후 하나 이상의 칼럼이 테이블에 존재해야함.
  - MODIFY 칼럼 : 데이터 유형, 디폴트 값, NOT NULL 제약조건에 대한 변경.
    - 칼럼의 크키를 늘릴 수는 있지만, 줄이지 못함.
    - 칼럼이 NULL값만 존재 or 테이블에 아무 행이 없다면 줄일 수 있다.
    - NULL값이 없을 때만 NOT NULL 설정 가능.
  - RENAME 칼럼, 테이블 : 이름 변경.
  - DROP CONSTRAINT, TABLE : 제약조건, 테이블 삭제
  - TRUNCATE 테이블 : 테이블의 행만 모두 삭제
  
<br>
<br>

### DML
<br>

- INSERT
  - INSERT INTO 테이블명 (COLUMN_LIST) <- COLUMN_LIST가 있는 경우 해당 값 매핑해서 입력. 정의 안한 칼럼엔 NULL, DEFAULT 값 입력. 단, PK나 Not NULL 칼럼은 포함 되어야 한다. NULL을 명시해도됨.
  
- UPDATE 테이블명
  SET 수정 칼럼명 = 수정 칼럼값
  값을 일괄 수정...?

- DELETE (FROM) 테이블명
  디폴트는 전체 데이터 삭제.

DML은 실시간으로 테이블에 영향을 끼치지 않고, 메모리 버퍼에 올려놓고 작업을 하다 commit을 하여 transaction을 종료 시킨다. SQL server의 경우 auto commit이기 때문에 commit 할 필요가 없다.

전체 데이터를 삭제하는 경우, 시스템 활용 측면에서 삭제된 데이터를 로그로 저장하는 DELETE TABLE보다 시스템 부하가 적은 TRUNCATE TABLE을 권장함. 

하지만 ROLLBACK이 불가하므로 주의해야한다. 근데 또 SQL SERVER는 TRUNCATE TABLE로 삭제해도 롤백 할 수가 있다..... 싯팔


- SELECT 칼럼1, 칼럼2, ...
  FROM 테이블명
  
  DISTINCT로 해당 칼럼의 고유값(값의 종류)를 볼 수 있다.
  *에스터리스트는 모든 칼럼을 호출 가능.

  - 산술 연산자
    - NUMBER와 DATE 자료형에 대해 적용된다. 괄호를 포함하여 연산이 가능하다.
    - 기존 칼럼에 대해 새로운 의미를 부여한 것이므로 적절한 ALIAS를 새롭게 부여하는 것이 좋다.
    - (), *, /, +, -, ROUND(arg1, arg2)

  - 합성 연산자
    - 문자열 합성.(CONCATENATION)
    - +, ||로 표시

<br>
<br>

### TCL
<br>

2-3 TCL(TRANSACTION CONTROL LANGUAGE)

- 트랜잭션 개요
  트랜잭션은 데이터베이스의 논리적 연산단위이다. 트랜잭션이란 밀접히 관련되어 분리될 수 없는 한 개 이상의 데이터베이스 조작을 가리킨다. 하나의 트랜잭션에는 하나 이상의 SQL문장이 포함된다.
-> 트랜잭션은 분할할 수 없는 최소의 단위. 그렇기 때문에 전부 적용하거나 전부 취소.


올바르게 반영된 데이터를 데이터베이스에 반영시키는 것을 COMMIT
트랜잭션 시작 이전 상태로 되돌리는 것을 ROLLBACK
중간 저장 가능 SAVEPOINT

- 트랜잭션의 특성
  - 원자성(atomicity) : 정의된 연산들은 모두 실행되던지 아니면 전혀 실행되지 않는지 둘 중 하나.
  - 일관성(consistency) : 트랜잭션 전 후로 내용에 잘못이 있으면 안 된다.
  - 고립성(isolation) : 트랜잭션 실행도중 다른 트랜잭션의 영향을 받으면 안됨.
  - 지속성(durability) : 트랜재션이 수행되면 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적 저장.

- COMMIT, ROLLBACK 하기전 데이터 상태
  - memory buffer에만 영향
  - select 문으로 결과 확인 가능
  - 다른 사용자는 현재 사용자가 수행한 명령의 결과를 볼 수 없다.
  - 변경된 행은 잠금이 설정되서 다른 사용자가 변경할 수 없다. <- 동시에 수정이 안되는듯?

COMMIT - 트랜잭션 완료

- COMMIT 이후
  - 변경 사항이 데이터베이스에 반영, 이전 데이터는 영원히 복구 불가
  - 모든 사용자는 결과를 볼 수 있다.
  - 관련된 행에 대한 잠금이 풀리고, 다른 사용자들이 행을 조작할 수 있게 된다.


- 3가지 트랜잭션
  1. AUTO COMMIT - DML,DDL을 수행할 때마다 COMMIT, 오류발생하면 자동으로 ROLLBACK. DBMS가 트랜잭션을 모두 컨트롤
  2. 암시적 트랜잭션 - 사용자가 명시적으로 COMMIT, ROLLBACK으로 트랜잭션 처리. 트랜잭션 시작만 DBMS가 처리.
  3. 명시적 트랜잭션 - 트랜잭션 시작과 끝 모두 사용자가 명시적으로 지정. BEGIN TRANSACTION으로 시작, COMMIT TRANSACTION, ROLLBACK TRANSACTION으로 종료. 롤백 경우 비긴 시점까지 롤백됨.

- 롤백의 효과
  - 데이터 무결성 보장
  - 영구적인 변경을 하기 전에 데이터의 변경 사항 확인 가능
  - 논리적으로 연관된 작업을 그룹핑하여 처리 가능

- SAVEPOINT
  - 여러개의 저장점 생성가능.
  - 지정한 저장점을 명시하여 롤백
  - 미래방향으로 되돌릴 수가 없다. <- 더 과거로 가면 그 상태에선 미래 저장점을 알리가 없다는 논리.


데이터 변경: 입력, 수정, 삭제에 데이터 무결성을 보장하는 것이 커밋과 롤백이다.
DDL 문장은 자동으로 커밋된다.
DML 문장 이후 커밋 없이 DDL문장 실행되면 자동으로 커밋됨.
데이터 베이스를 정상적으로 접속 종료시 자동 커밋
이상 종료시 자동으로 롤백됨.


<br><br>

### WHERE
<br>


WHERE 절에는 두 개 이상의 테이블에 대한 조인 조건을 기술하거나 결과를 제한하기 위한 조건을 기술할 수도 있다.


SELECT 칼럼 FROM 테이블 WHERE 조건식.

- 조건식
  - 칼럼명
  - 비교 연산자
  - 문자, 숫자, 표현식
  - 비교 칼럼명 (JOIN 사용시)

- 연산자
  - 비교연산자 : =, >=, >
  - SQL 연산자 : BETWEEN a and b /이상, 이하, IN (list) / 리스트 값 중 어느 하나라도 일치.
    LIKE '비교문자열'/ 비교문자열과 형태가 일치하면 된다...?
    IS NULL / NULL값인 경우
  - 논리 연산자 : AND, OR, NOT
  부정은 NOT을 붙이면,,

WHERE절 사용 예시 

SELECT PLAYER_NAME 선수이름, POSITION 포지션, BACK_NO 백넘버, HEIGHT 키
FROM PLAYER
WHERE ~

  WHERE POSITION = 'MF'; //포지션이 미드필더
  WHERE HEIGHT >= 170; //키가 170 이상인사람
  ('170' 으로 쿼리를 날려도 괜찮다.)

- SQL 연산자
  예약되어 있는 연산자이다.
  - IN ('K02', ...)
  - LIKE "MF"
  - BETWEEN 170 AND 180
  
  IN 연산자를 통하면 다양한 경우를 출력 할 수 있다.
  ex.
  SELECT ENAME, JOB, DEPTNO
  FROM EMP
  WHERE (JOB, DEPTNO) IN (('MANAGER', 20), ('CLERK', 24));

  LIKE 연산자는 %, _를 통해 정규표현식으로 사용 하여 쿼리를 보낼 수 있다.

논리 연산자는 AND가 OR보다 먼저 처리된다.

- 연산자 우선순위
  괄호
  NOT 연산자
  비교연산자, SQL 비교연산자.
  AND
  OR


- TOP 절
  TOP(Expression) [PERCENT] [WITH TIES]
  Expression: 반환할 행의 수
  PERCENT : 행의 수 대신 그 퍼센트 
  WITH TIES : ORDER BY 절이 지정된 경우에만 사용할 수 있다. 


<br><br>

### 함수
<br>

함수는 입력 값이 많아도 출력은 하나만 된다는 M:1 관계다. (1:1도 포함.)

- 내장 함수
  - 단일 행함수
  - 다중행 함수
    - 집계 함수
    - 그룹 함수
    - 윈도우 함수


- SELECT, WHERE, ORDER BY 절에 사용 가능하다.
- 여러 인자(Argument)를 입력해도 단 하나의 결과만 리턴한다.
- 함수의 인자로 상수, 변수, 표현식이 사용 가능하다.
- 특별한 경우가 아니면 함수의 인자로 함수를 사용하는 함수의 중첩이 가능하다.



- 문자형 함수
  - LOWER, UPPER, ASCII, CHAR, CONCAT, SUBSTR, LTRIM, RTRIM, TRIM

오라클은 사용자 테이블이 필요 없는 SQL 문장의 경우에도 필수적으로 DUAL이라는 테이블을 FROM절에 지정한다. SQL Server의 경우 SELECT 절만으로도  SQL문장이 수행 가능하도록 정의하였기 때문에 DUAL이란 DUMMY 테이블이 필요 없다.


- 숫자형 함수
  - ABS, SIGN, MOD, CEILING, FLOOR, ROUND, TRUNC, ...


- 날짜형 함수
  - SYSDATE/GETDATE()
  - EXTRACT('YEAR' from d)/DATEPART('YEAR', d)
  - TO_NUMBER(TO_CHAR(d, 'YYYY'))/YEAR(d)

날짜 + 숫자 = 날짜
날짜 + 날짜 = 일수(숫자)

- 변환형 함수
  - 명시적(Explicit) : 데이터 변환형 함수로 데이터 유형을 변환하도록 명시해 주는 경우
  - 암시적(Implicit) : 데이터베이스가 자동으로 데이터 유형을 변환하여 계산하는 경우

암시적은 성능 저하가 발생하거나, 계산하지 않는 경우가 있어서 명시적 사용이 권고됨.

- 명시적 변환형 함수
  Oracle
  - TO_NUMBER(문자열)
  - TO_CHAR(숫자, FORMAT)
  - TO_DATE(문자열, FORMAT)
  SQL Server
  - CAST(expression As data_type)
  - Convert(data_type [(length)], expression ,[style])

- CASE 표현
  - IF-THEN-ELSE 논리로 비교 연산 기능을 보완한다.
  CASE
    SIMPLE/SEARCHED_CASE_EXPRESSION 조건
    WHEN ~ THEN ~
    WHEN ~ THEN ~
    WHEN ~ THEN ~
    ...
    ELSE 표현절
  END
  조건이 맞으면 THEN절을 수행하고, 맞지 않으면 ELSE 절을 수행한다.
  CASE함수도 중첩해서 사용할 수 있다.



- NULL 관련 함수
  - NVL/ISNULL 함수
    널 값은 아직 정의되지 않은 값으로 0 또는 공백과 다르다.
    널 값을 포함하는 연산의 경우 결과 값도 널 값이다.
    -> NVL/ISNULL을 사용하면 NULL이 아닌 결과값을 얻을 수 있다.

    NVL(표현식1, 표현식2)/ISNULL(표현식1, 표현식2)
    표현식1의 결과값이 NULL이면 표현식2의 값을 출력한다.
    단, 표현식1과 표현식2의 결과 데이터 타입이 같아야 한다.

    SELECT ENAME 사원명, SAL 월급, COMM 커미션,
    (SAL * 12) + COMM 연봉A,          -> COMM이 널값이면 SAL이 있어도 NULL값
    (SAL * 12) + NVL(COMM, 0) 연봉B   -> COMM이 없어도 연봉을 찍을 수 있다.
    FROM EMP;


    SELECT MRG              FROM EMP WHERE ENAME = "KING";
    SELECT NVL(MRG, 9999)   FROM EMP WHERE ENAME = "KING"; -> 행을 찍을 수 있다.
    
    SELECT MAX(MGR)             MGR FROM EMP WHERE ENAME ='JSC'; -> 공집합
    SELECT NVL(MAX(MGR), 9999)  MGR FROM EMP WHERE ENAME ='JSC'; -> 공집합도 찍는다.

  - NULLIF 함수
    NULLIF(EXPR1, EXPR2)
    EXPR1이 EXPR2와 같으면 NULL, 같지 않으면 EXPR1을 리턴한다. 특정값을 NULL로 대체하는 경우 사용.

    SELECT ENAME, EMPNO, MGR, NULLIF(MGR, 7698) NUIF FROM EMP;
  
  - COALESCE 함수
    COALESCE(EXPR1, EXPR2, ...)
    임의의 개수 EXPR에서 NULL이 아닌 최초의 EXPR을 나타낸다.
    만일 모든 표현식이 NULL이라면 NULL을 리턴한다.
    
    SELECT ENAME, COMM, SAL, COALESCE(COMM, SAL) COAL FROM EMP;
    
  

<br><br>

### GROUP By and Having

1. 집계 함수(Aggregate Function)
  여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 다중행 함수이다.

- 여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 함수이다.
- GROUP BY 절은 행들을 소그룹화 한다.
- SELECT 절, HAVING 절, ORDER BY 절에 사용할 수 있다.

COUNT(*)      전체 행 출력
COUNT(표현식)  표현식이 NULL값이 아닌 행을 출력
SUM 
AVG 
MAX 
MIN 
STDDEV 
VARIAN 
SELECT COUNT

    SELECT COUNT(*) "전체 행수", COUNT(HEIGHT) "키 건수", 
    MAX(HEIGHT) 최대키, MIN(HEIGHT) 최소키, ROUND(AVG(HEIGHT), 2) 평균키
    FROM PLAYER;

    COUNT(*) -> 전체 칼럼이 NULL인 행은 존재할 수 없기 때문에 전체 행의 개수를 출력한다.



### ORDER BY

WHERE 절을 통해 조건에 맞는 데이터를 조회했지만 테이블에 1차적으로 존재하는 데이터 이외의 정보, 예를 들면 각 팀별로 선수가 몇 명인지, 선수들의 평균 신장과 몸무게가 얼마나 되느니, 각 팀에서 가장 큰 키의 선수가 누구인지 등 2차 가공 정보도 필요하다. ... . . . . .

- 예시
SELECT [DISTINCT] 칼럼명 [ALIAS 명]
FROM 테이블명
[WHERE ~]
[GROUP BY 칼럼/표현식]
[HAVING 그룹조건식]:

- GROUP BY 절을 통해 소그룹별 기준을 정한 후, SELECT 절에 집계 함수를 사용
- 집계 함수의 통계 정보는 NULL 값을 가진 행을 제외하고 수행
- GROUP BY 절에서는 SELECT 절과는 달리 ALIAS 명을 사용 불가.
- 집계 함수는 WHERE 절에는 올 수 없다. GROUP BY 절보다 WHERE 절 먼저 수행됨.
- WHERE 절은 전체 데이터를 GROUP으로 나누기 전에 행들을 미리 제거시킨다.
- HAVING 절은 GROUP BY 절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있다.
- GROUP BY 절에 의한 소그룹별로 만들어진 집계 데이터 중, HAVING 절에서 제한 조건을 두어 조건을 만족하는 내용만 출력한다.
- HAVING 절은 일반적으로 GROUP BY 절 뒤에 위치한다.


SELECT  POSITION 포지션, COUNT(*) 인원수, COUNT(HEIGHT) 키대상,
        MAX(HEIGHT) 최대키, MIN(HEIGHT) 최소키, ROUND(AGV(HEIGHT), 2) 평균키
FROM    PLAYER GROUP BY POSITION;


- HAVING 절
  
SELECT  POSITION 포지션 ROUND(AVG(HEIGHT), 2) 평균키
FROM    PLAYER WHERE AVG(HEIGHT) >= 180
GROUP BY POSITION;
WHERE AVG(HEIGHT) >= 180 *

ERROR : 집계함수는 허가되지 않는다.

-> WHERE 절에는 집계 함수는 사용할 수 없다. 

FROM 절에 정의 된 집합의 개별 행에 WHERE 절의 조건절이 먼저 적용되고, 이 행이 GROUP BY절의 대상이 된다. 그 다음 결과 집합의 행에 HAVING 조건절이 적용된다.

WHERE은 SELECT을 하는 조건이고, 집계함수는 SELECT된 집합에 적용이 가능하다고 한다....
따라서 HAVING절을 활용해 주자.

SELECT POSITON 포지션, ROUND(AVG(HEIGHT),2) 평균키
FROM PLAYER
GROUP BY POSITION
HAVING AVG(HEIGHT) >= 180;

-> GROUP BY 절과 HAVING 절의 순서를 바꾸어도 동작하지만 논리상 순서를 지켜주자.

GROUP BY 소그룹의 데이터 중 일부만 필요한 경우, 

- GROUP BY 전 WHERE 절에서 거르거나
- GTOUP BY 후 HAVING 절에서 거르는 방법이 있다. 
자원 효율을 생각해서 되도록 소그룹화 하기전에 걸러주자.

HAVING절은 SELECT 절에 사용되지 않은 칼럼이나 집계 함수가 아니더라도 GROUP BY 절의 기준 항목이나 소그룹의 집계 함수를 이용한 조건을 표시할 수 있다.


- ORDER BY 절
  ORDER BY 절은 SQL 문장으로 조회된 데이터들을 다양한 목적에 맞게 특정 칼럼을 기준으로 출력하는데 사용.
  ORDER BY절에 ALIAS 명이나 칼럼 순서를 나타내는 정수도 사용 가능. 정렬 방식을 지정핮 않으면 기본이 오름차순이다.



- SELECT 문장 실행 순서

5. SELECT 칼럼명 [ALIAS명]
1. FROM 테이블명
2. [WHERE 조건식]
3. [GROUP BY 칼럼이나 표현식]
4. [HAVING 그룹조건식]
6. [ORDER BY 칼럼이나 표현식 [ASC/DESC]]


### JOIN

지금까지 하나의 테이블에서 데이터를 출력. -> 두 개 이상의 테이블 들을 연결/결합 하여 출력하자

일반적인 경우 행들은 PK/FK 값의 연관에 의해 JOIN이 성립된다. 하지만 어떤 경운에는 PK/FK관계가 없어도 논리적인 값들의 연관만으로 JOIN이 성립 가능하다.

주의할 점은 FROM절에 여러 테이블이 나열되더라도 SQL에서 데이터를 처리 할 때에는 두 개씩 조인 처리를 한다.


- EQUI JOIN
  등가 조인은 두 개의 테이블 간에 칼럼 값들이 서로 정확하게 일치하는 경우에 사용되는 방법으로 대두분 PK/FK의 관계를 기반으로 한다. 

  SELECT 테이블1.칼럼명, 테이블2.칼럼명, ...
  FROM 테이블1, 테이블2
  WHERE 테이블1.칼럼명1 = 테이블2.칼럼명2;
  /ON  테이블1.칼럼명1 = 테이블2.칼럼명2;

  계층적인 표현을 활용하자.

조인 조건에 맞는 데이터만 출력하는 INNER JOIN에 참여하는 대상 테이블이 N개라고 했을 때, N개의 테이블로부터 필요한 데이터를 조회하기 위해 필요한 최소 JOIN 조건은 N-1개 이다.

선수와 팀명을 맞추는 예시

SELECT    PLAYER.PLAYER_NAME, PLAYER.BACK_NO, PLAYER.TEAM_ID,
          TEAM.TEAM_NAME, TEAM.REGION_NAME
FROM      PLAYER, TEAM
WHERE     PLAYER.TEAM_ID = TEAM.TEAM_ID

ALIAS를 적용하여 줄일 수 있다. 추가 조건을 넣어도 된다.
근데 FROM 절에서 테이블명을 사용한다면 WHERE 절과 SELECT 절에는 꼭 ALIAS를 써야한다.

SELECT    P.PLAYER_NAME, P.BACK_NO, P.TEAM_ID,
          T.TEAM_NAME, T.REGION_NAME
FROM      PLAYER P, TEAM T
WHERE     P.TEAM_ID = T.TEAM_ID AND P.POSIITON = 'GK'
ORDER     BY P.PACK_NO;


- Non EQUI JOIN
  비등가 조인은 두 개의 테이블 간 칼럼 값들이 서로 정화하게 일치하지 않는 경우에 사용된다.

SELECT  테이블1.칼럼명, 테이블2.칼럼명, ...
FROM    테이블1, 테이블2
WHERE   테이블1.칼럼명1 BETWEEN 테이블2.칼럼명1 AND 테이블2.칼럼명2;

급여등급를 조인으로 가져와보자~

SELECT  E.ENAME 사원명, E.SAL 급여, S.GRADE 급여등급
FROM    EMP E, SALGRADE S
WHERE   E.SAL BETWEEN S.LOSAL AND S.HISAL




- 3개 이상 TABEL JOIN

선수 - 팀 - 전용 구장
이거 조인해보자~

SELECT  P.PLAYER_NAME 선수명, P.POSITION 포지션, T.REGION_NAME 연고지,
        T.TEAM_NAME 팀명, S.STADIUM_NAME 구장명
FROM    PLAYER P, TEAM T, STADIUM S
WHERE   P.TEAM_ID = T.TEAM_ID AND T.STADIUM_ID = S.STADIUM_ID
ORDER   BY 선수명;


조인이 필요한 기본적인 이유는 데이터를 정규화 해놔서.

불필요한 데이터의 정합성을 확보하고 이상현상을 제거하기 위해 정규화, 테이블을 분활해 놔서 조인해야 보기 조타.

