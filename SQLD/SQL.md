# SQL 정리

<hr/><hr/>
## 데이터 베이스 종류

1. 계층형 데이터베이스 : 트리 형태에 데이터를 저장하고 관리하며, 1:N 관계
2. 네트워크형 데이터베이스 : 오너와 멤버 형태로 데이터 저장, M:N 관계
3. 관계형 데이터베이스 : 릴레이션에 데이터를 저장하고 관리
   - 연산 종류
     - 집합 연산
       1. 합집합(Union) 
       2. 차집합(Difference)
       3. 교집합(Intersection)
       4. 곱집합(Cartesian product)
     - 관계 연산
       1. 선택연산(Selection) : 조건에 맞는 튜플만 조회
       2. 투영연산(Projection) : 조건에 맞는 속성만 조회
       3. 결합연산(Join) : 릴레이션의 공통된 속성을 사용해서 새로운 릴레이션을 만들어 냄
       4. 나누기 연산(Division) :  한 릴레이션에서 다른 릴레이션의 속성을 선택하는 것

<hr/>
### SQL 종류

- 표준 
  - ANSI/ISO SQL 표준 : INNER JOIN, NATUAL JOIN, USING 조건, ON 조건절을 사용한다.
  - ANSI/ISO SQL3 표준 : DBMS 벤더별로 차이가 있었던, SQL을 표준화하여 제정했다.
  
- 종류
  - 데이터 정의 언어(DDL : Data Definition Language)
    - 구조를 정의하는 언어
    - crate, alter, drop, rename
    
  - 데이터 조작 언어(DML : Data Manipulation Language)
    - 입력, 수정, 삭제, 조회
    - insert, update, delete, select
    
  - 데이터 제어 언어(DCL : Data Control Language)
    - 권한을 부여하거나 회수
    - grant, revoke
    
  - 트랜잭션 제어 언어(TCL : Transaction Control Language)
    
    - commit : 변경한 데이터를 데이터베이스에 반영한다.
    - rollback : 데이터에 대한 변경 사용을 모두 취소하고 트랜잭션을 종료한다.
    - savepoint : 트랜잭션을 분할하여 관리하는 것으로, 지정된 위치까지 롤백을 할 수 있다.
    
      
  
- 트랜잭션 
  - 데이터베이스 작업을 처리하는 단위 
  - 종류
    1. 원자성 : 처리가 끝나지 않았을 경우 전혀 이루어지지 않은 것과 같아야한다
    
    2. 일관성 : 실행 결과 모순되지 않아야 하며, 일관성이 유지되어야 한다.
    
    3. 고립성 : 연산 도중 다른 트랜잭션이 접근 할 수 없다.
    
    4. 연속성 : 트랜잭션이 성공하면 그 결과는 영구적 보장이 되어야 한다.
    
       
  
- SQL 실행 순서
  - DCL -> DDL ->  DML
  
  - 파싱 -> 실행 -> 인출
  
    
  
- 외래키(PK)

  - 테이블을 생성할 때 FK를 정의한다.

  - FK가 정의된 테이블이 자식 테이블이다.

  - 참조되는 테이블을 부모 테이블이라고 한다.

  - 부모 테이블은 미리 생성되어 있어야 한다.

  - 부모 테이블의 참조되는 컬럼에 존재하는 값만을 입력 할 수 있다.

  - 부모 테이블은 FK로 인해 삭제가 불가능하다.

  - REFERENCES : 참조할 부모 테이블과 부모 테이블에 있는 컬럼을 정의한다.

  - ON DELETE CASCADE : 참조되는 부모 테이블의 행에 대한 DELETE를 허용한다.

    - 부모 테이블의 행이 지워지면 자식 테이블의 행도 같이 지워진다.

  - ON DELETE SET NULL : 참조되는 부모 테이블의 행에 대한 DELETE를 허용한다.

    - 부모 테이블의 행이 지워지면 자식 테이블의 행은 NULL 값으로 설정된다.

  - 데이터 타입이 반드시 일치해야 한다.

  - 참조되는 컬럼은 PK이거나 UK(Unique key)만 가능하다.

  - 외부키, 참조키, 외부 식별자 등으로 불린다.

<hr/>
### DDL( Data Definition Language)

- 관리 SQL문
  - create tabel : 새 테이블을 생성하며, 기본키, 외래키, 제약사항 등을 지정할 수 있다.
  
  - alter tabel : 테이블을 변경한다.(속성 추가, 변경, 삭제)
  
  - drop table : 테이블을 완전히 삭제한다.
  
    
  
- 제약조건(constraint) :  특정 컬럼에 설정하는 제약 

  - ON DELETE CASCADE : 참조되는 부모 테이블의 행에 대한 DELETE를 허용한다.

     - 부모 테이블의 행이 지워지면 자식 테이블의 행도 같이 지워진다.

  - ON DELETE SET NULL : 참조되는 부모 테이블의 행에 대한 DELETE를 허요한다.

    - 부모 테이블의 행이 지워지면 자식 테이블의 행은 NULL 값으로 설정된다.

  ```mysql
    /*외래키 지정 방법(둘이 같은 방법)*/
    /*1*/
    dno varchar2(2),
    constraint emp_eno_fk foreign key (dno) references dept (dno) ON DELETE CASCADE
    /*2*/ 
    dno varchar2(2) constraint emp_dno_fk references dept (dno) ON DELETE CASCADE
    /*테이블 생성문 안에서 사용하므로 ;사용 x, 마지막이 아니라면 , 사용*/
  ```


- 테이블 변경

  ```mysql
  /*테이블 이름 변경*/
  alter table EMP rename to NEW_EMP;
  
  /*컬럼 추가*/
  alter table EMP add(age number(2) defult 1);
  	
  /*컬럼 변경*/
  alter table EMP modify (ename varchar2(20) not null);
  
  /*칼럼 삭제*/
  alter table EMP drop column age;
  
  /*칼럼명 변경*/
  alter table EMP rename column ename to new_ename;
  
  /*테이블 삭제 = 로그를 남기지 않음, 스키마도 지움*/
  drop table emp
  drop table emp cascade constraint; #참조된 제약사항까지 모두 삭제
  ```

 - 뷰(View)

   - 테이블로 부터 유도된 가상 테이블
   - 실제로 값을 가진 것이 아닌 참조해서 원하는 칼럼만 조회
   - 참조한 테이블이 변경되면 뷰도 변경됨
   - 장점
     - 특정 칼럼만 조회하기에 보안성이 향상됨
     - 데이터 관리가 간단
     - select문이 간단해짐
   - 단점
     - 삽입, 수정, 삭제 연산이 제한됨
     - 데이터 구조를 변경 할 수 없음


```mysql
/*뷰 생성*/
create view T_EMP as select * from EMP;

/*뷰 조회*/
select * from T_EMP;

/*뷰 삭제*/
drop view T_emp;
```

<hr/>
### DML(Data Manipulation Language)

```mysql
/*
테이블에 데이터 추가, insert를 한다고 바로 저장 되는 것이 아님
최종적으로 데이터를 저장하려면 TCL문인 commit을 실행해야함, auto commit으로 설정된 경우 자동 저장 됨
*/ 
insert into EMP(eno, ename, ...) values (100, '가나다'); 

/*데이터를 조회하면서 삽입하기*/
insert into t_dept select * from dept;

/*
데이터 수정 
조건문에 해당하는 행 수 만큼 변경되며, 조건을 입력하지 않으면 모두 변경됨
*/
update EMP set ename = '가나' where eno = 100;

/*
데이터 삭제
조건문에 해당하는 행 수 만큼 삭제되며, 조건을 입력하지 않으면 모두 삭제됨
*/
delete from EMP where eno = 100;

/*용량을 초기화 시키면서 모두 삭제, 로그를 남기지 않음, 스키마는 남김*/
truncate from EMP;
```

```mysql
/*데이터 조회*/
select * from EMP;

/*문자를 결합하여 조회*/
select enmae || '님' from EMP;

/*오름차순 내림차순 조회*/
 select * from EMP order by ename, eno desc;
 
 /*인덱스를 이용한 내림차순 조회*/
 select /*+ index_desc(a) */ from EMP a;
 
 /*중복 안보이게 조회*/
 select distinct eno from EMO
 
 /*별칭 사용*/
 select ename as '이름' from EMP a;
```



<hr/>
### Nologging

- 버퍼 캐쉬라는 메모리 영역을 생략하고 기록하여 성능을 향상시킬 수 있는 방법이다.

```mysql
alter table EMP nologging
```

<hr/>
### Where문

```mysql
/*Like문*/
select * from EMP where ename like 'test'; # test인 모든 데이터
select * from EMP where ename like 'test%'; # test로 시작하는 모든 데이터
select * from EMP where ename like '%test'; # test로 끝나는 모든 데이터
select * from EMP where ename like '%test%'; # test가 있는 모든 데이터
select * from EMP where ename like 'test_'; # test로 시작하고 한 글자만 더 있는 모든 데이터
```

```mysql
/*Between문*/
select * from EMP where eno between 100 and 200; 
select * from EMP where eno not between 100 and 200;
```

```mysql
/*IN문(or 의미)*/
select * from EMP where eno in (10,20);
select * from EMP where (eno,ename) in ((10,'가'),(20,'나'));
```

```mysql
/*Null값 조회*/
/*
	null = 모르는 값
	null과 비교하면 알 수 없음을 반환
	null과 연산을하면 null
	null과 비교 연산을 하면 false
	count, sum 등 연산시 연산에서 제외 됨
*/
select * from EMP where eno is null;
select * from EMP where eno is not null;
```

```mysql
/*구조 복사*/
select * from EMP where 1=1; # 1=1은 참을 의미하며, 전체구조와 내용 복사

select * from EMP where 1=2; # 1=2는 거짓을 의미하며, 구조만 복사
```



```mysql
/*Null 관련 함수*/
/*NVL함수 = null이면 다른 값으로 바꾸는 함수*/
NVL(eno,0) # eno 칼럼이 null이면 0

/*NVL2함수 = NVL함수와 DECODE를 하나로 만든 함수*/
NVL2(eno,0,1) # eno 칼럼이 null이면 0, 아니면 1

/*NullIf함수 = 두개의 값이 같으면 null, 아니면 첫번째 값을 반환*/
Nullif(exp1,exp2)

/*COALESCE*/
COALESCE(eno,2,1) # null이 아닌 첫번재 값을 반환하며 모두 널이면 null 반환
```

<hr/>
### GROUP BY문

```mysql
/*Group By문 = 행을 그룹화함*/
select eno, sum(pay) from EMP group by eno;

/*Having문 = group by에 조건을 검, where절을 사용하면 group대상에서 제외 됨*/
select eno , sum(pay) from EMP group by eno having sum(pay) > 10000;
select deptno, sum(pay) from EMP where eno between 1000 and 1003 group by deptno;
```

```mysql
/*Count 함수*/
select count(*) from EMP; # null을 포함하여 행 수를 계산
select count(eno). from EMP; # null을 제외하고 행 수를 계산
```

#### 집계함수 종류

- STDDEV() : 표준편차를 계산한다.

- VARIAN() : 분산을 계산한다.

  

#### select문 실행 순서

- from - where - group by - having - select - order by

<hr/>
#### 형변환

- 명시적 형변환 : 형변환 함수를 사용하여 데이터 타입을 일치시키는 것
- 암시적 형변환 : 형변환을 하지 않은 경우 자동으로 형변환하는 것
- 형변환 함수
  1. TO_NUMBER(str) : 문자열을 숫자로 변경한다.
  2. TO_CHAR(숫자 혹은 날짜, [format]) : 지정된 format의 문자로 변환한다.
  3. TO_DATE(문자열, format) : 지정된 format의 날짜형으로 변환한다.

<hr/>
### 내장형 함수

- 문자형 함수

  - ACSII(문자) : 문자 혹은 숫자를 아스키 코드 값으로 변환한다.

  - CHAR(ACSII 코드 값) : 아스키 코드 값을 문자로 변환한다.

  - SUBSTR(문자열,m,n) : 문자열에서 m번째에서 n개를 자른다.

  - CONCAT(문자열1, 문자열2) : 두 문자열을 결합한다.

  - LOWER(문자열) : 영문자를 소문자로 변환한다.

  - UPPER(문자열) : 영문자를 대문자로 변환한다.

  - LENGTH(문자열) : 공백을 포함한 길이를 알려준다.

  - LTRIM(문자열, 지정문자) : 왼쪽의 지정된 문자를 삭제한다.

  - RTRIM(문자열, 지정문자) :  오른쪽의 지정된 문자를 삭제한다. 

  - TRIM(문자열, 지정문자) : 양쪽의 지정된 문자를 삭제한다.

    

- 날짜형 함수

  - SYSDATE : 오늘의 날짜를 날짜 타입으로 알려준다.

  - EXTRACT('YEAT' | 'DAY' from 날짜변수) : 날짜에서 원하는(년, 월, 일)을 조회한다.

    

- 숫자형 함수

  - ABS(n) : 절대값을 돌려준다.
  - SIGN(n) : 양수, 음수, 0을 구별한다. (1,0,-1) 
  - MOD(n1, n2) : n1 % n2, %를 사용해두 됨
  - CELL/CELLING(n) : n보다 크거나 같은 최소의 정수를 돌려준다. (소수점 올림)
  - FLOOR(n) : n보다 작거나 같은 최대의 정수를 돌려준다. (소수점 내림)
  - ROUND(숫자, m) : 소수점 m번째에서 반올림한다. (소수점 반올림)
  - TRUNC(숫자, m) : 소수점 m번째에서 절삭한다. (소수점 버림)

<hr/
### DUAL 테이블

- 오라클 데이터베이스에 의해서 자동으로 생성되는 테이블
- 오라클은 기본적으로 DUAL 테이블을 더미 테이블로 존재한다.

<hr/>
### DCODE문과  CASE문

```mysql
/*DECODE(IF)*/
DECODE(eno,100,'TRUE','FALSE') from EMP; 

/*CASE(IF ELSE)*/
select case 
		when eno = 100 then 'a'
		when eno = 200 then 'b'
		else 'c'
	end
from EMP;
```

<hr/>
### ROWNUM 과 ROWID

- ROWNUM 
  - 행 수를 제한할 때 사용
  - ROWNUM을 사용해서는 한 개의 행을 가지고 올 수 있음
  - 여러행을 가지고 올 때는 인라인 뷰(from절에 select문 사용)를 사용해야함

```mysql
/*인라인 뷰를 사용하여 행 수 제한*/
select * from (select ROWNUM r, ename from EMP) where r <= 5;

SELECT FRIST_NAME,JOB_ID
	FROM (
		SELECT FIRST_NAME, JOB_ID
		FROM HR.EMPLOYEES
		ORDER BY SALARY
		)
WHERE ROWNUM <= 10;
```

- ROWID
  - 데이터를 구분할 수 있는 유일한 값
  - 구조

| 구조          | 길이  | 설명                                    |
| ------------- | ----- | --------------------------------------- |
| 오브젝트 번호 | 1~6   | 해당 오브젝트가 속해 있는 값            |
| 상대 파일번호 | 7~9   | 테이블스페이스에 속행있는 상대 파일번호 |
| 블록 번호     | 10~15 | 어느 블록에 있는지 알려줌               |
| 데이터 번호   | 16~18 | 데이터 블록에 저장되어 있는 순서를 의미 |

ex)  AAAGHSAABAAALLBAAA : eno

<hr/>
### DCL(Data Control Language)

- 권한 종류
  1. SELECT
  2. INSERT
  3. UPDATE
  4. DELETE
  5. REFERENCES
  6. ALTER
  7. INDEX
  8. ALL
  9. WITH GRANT OPTION : 특정 사용자게에 권한을 부여할 수 있는 권한을 부여, 권한 취소 시 모두 권한 회수
  10. WITH ADMIN OPTION : 테이블에 대한 모든 권한을 부여, 권한 취소시 당사자만 취소 됨

```mysql
grant select, update on EMP to tester;
grant all on EMP to tester with grant option;

revoke select on EMP from tester;
```

<hr/
<hr/>
# SQL 활용

<hr/><hr/>
### 조인(Join) 

- 여러 개의 릴레이션을 사용하여 새로운 릴레이션을 만드는 과정

- EQUI(등가) 조인

  ```mysql
  /*EQUI join*/
  select * from EMP, DEPT where EMP.deptno = DEPT.deptno;
  
  select * from EMP, DEPT where EMP.deptno = DEPT.deptno
  	and EMP.ename like '임%' 
  	order by enam;
  	
  /*Inner join*/
  select * from EMP inner join DEPT on EMP.deptno = Dept.deptno;
  
  select * from EMP, DEPT where EMP.deptno = DEPT.deptno
  	and EMP.ename like '임%'
  	order by ename;
  	
  /*Intersect 연산 = 교집합을 조회*/
  select deptno from EMP
  intersect
  select deptno from DEPT;
  ```

- Non-EQUI(비등가) 조인

  - 두 테이블간 조인하는 경우 '='이 아닌 '>',  '<' 등사용

  - 정확하게 일치하지 않는 것을 조인하는 것

    

- OUTER JOIN

  - 두 테이블 간에 교집합을 조회하고 한쪽 테이블에만 있는 데이터도 포함시켜서 조회
  - 왼쪽 테이블만 포함하면 Left Outer join
  - 오른쪽 테이블만 포함하면 Right Outer join
  - 양쪽 테이블 포함하면 Full Outer join

  ```mysql
  /*Full???*/
  select * from DEPT, EMP where EMP.dept.no (+)= DEPT.deptno;
  
  /*Left Outer join*/
  select * from DEPT left outer join EMP on EMP.dept.no = DEPT.deptno;
  
  /*Right Outer join*/
  select * from DEPT right outer join EMP on EMP.dept.no = DEPT.deptno;
  ```

- CROSS JOIN(Cartesian)

  - 조건 구 없이 2개의 테이블을 하나로 조인한다.
  - 카테시간 곱이 발생(4행 * 5행 = 20행)

  ```mysql
  select * from EMP cross join DEPT;
  ```

- UNION

  - 두 개의 테이블을 하나로 합치는 연산
  - 두 개의 테이블 칼럼 수, 칼럼의 데이터 형식 모두가 일치해야 가능
  - 하나로 합쳐지면서 중복된 데이터는 제거되고 정렬된다.

  ```mysql
  /*UNION*/
  select deptno from EMP
  union
  select deptno from EMP;
  
  /*UNION ALL = 중복을 제거하거나 정렬하지 않는다.*/
  select deptno from EMP
  union all
  select deptno from EMP;
  ```

- MINUS

  ```mysql
  select deptno from EMP
  minus
  select deptno from DEPT;
  ```


<hr/>
### 계층형 조회(Connect by)

- 계층형 데이터를 순차적으로 조회하는 것, 역방향 조회도 가능

```mysql
select max(level) from EMP # level = 계층
	start with mgr is null # 시작조건 
	connect by prior eno = mgr; # 조인 조건
```

<hr/>
### 서브쿼리(Subquery)

- select문에 다시 select문을 사용하는 sql문이다.
- from구에 select문을 사용하면 인라인 뷰이고, select문에 다시 사용하는 스칼라 서브쿼리가 있다.
- where구에 select문을 사용하면 서브쿼리라고 한다.

```mysql
select * from EMP where deptno=
	(select deptno from dept where deptno = 10);
```

- 서브쿼리 종류

  - 단일 행 서브쿼리 : 실행하면 한 행만 조회된다. ex) =, <, >

  - 다중 행 서브쿼리 : 실행하면 여러 행이 조회된다. 

    - IN : 비교조건이 서브쿼리의 결과 중 하나만 동일하면 참(OR조건)
- ALL : 메인쿼리와 서브쿼리의 결과가 모두 동일하면 참 / >ALL 최대값 / <ALL 최소값 반환
    - ANY :  비교조건이 서브쿼리 결과 중 하나 이상 동일하면 참 / >ANY 하나라도 크면 참 / <ANY 하나라고 작으면 참
- EXISTS : 메인쿼리와 서브쿼리의 결과가 하나라고 존재하면 참
    -  https://doorbw.tistory.com/222 

```mysql
/*IN*/
select ename, dname, pay from EMP, DEPT
where EMP.deptno = DEPT.deptno
	and EMP.empno # 2. empno를 비교해서 조건에 맞는 결과 반환
	in (select empno from EMP where pay >2000); # 1. 급여가 2000이상인 empno를 찾아 반환
	
/*ALL*/
select * from EMP where deptno <= ALL(20,30); # 20, 30보다 작거나 같으면 조회됨

/*EXISTS*/
select ename, dname, pay from EMP, DEPT
where EMP.deptno = DEPT.deptno
and exists (select 1 from EMP where pay > 2000); # pay>2000이 존재하면 true 반환

/*scala subquery*/
select ename as "이름", pay as "급여",
	(select avg(pay) from EMP) as "평균급여"
	from EMP where empno = 1000;

/*연관 subquery*/
select * from EMP a where a.deptno =
	(select eptno from dept b
    	where b.deptno  = a.deptno);
```

<hr/
### 그룹함수(Group Function)

```mysql
/*ROLLUP = group by 칼럼에 subtotal을 만들어 준다*/
select decode(deptno, null, '전체합계', deptno), # deptno가 null 이면 '전체합계', 아니먄 deptno 출력
	snm(pay)
	from EMP
	group by rollup(deptno); # 부서별 합계 및 전체합계가 계산된다.
	
/*두 가지 그룹핑*/
select deptno, job,
	sum(pay)
	from EMP
	group by rollup(deptno, job); # 부서별, 직업별 합계를 구함
```

```mysql
/*Grouping = rollupm cube, grouping set에서 생성되는 합계 값을 구분해줌*/
select deptno grouping(deptno),
	job, grouping(jop), # 0,1로 grouping된 값인지 아닌지 알려줌
	sum(pay)
	from EMP
	group by rollup(deptno, job);
	
/*DECODE and GROUPING*/
select deptno,
	decode(grouping(deptno),1,'전체합계') TOT,
	job,
	decode(grouping(job),1,'부서합계') T_DEPT,
	sum(pay)
	from EMP
	group by rollup(deptno, job);
	
/*GROUPING SETS = group by에 나오는 칼럼의 순서와 관계없이 개별적으로 처리*/
select deptno, job, sum(pay)
	from EMP
	group by grouping sets(deptno, job); # deptno, job의 순서가 바뀌어도 결과는 동일
									  # 각각의 그룹으로 합계를 계산
									  
/*CUBE = 결합 가능한 모든 집계를 계산*/
select deptno, job, sum(pay)
	from EMP
	group by cube(deptno, job); # 전체합계, 직업별, 부서별, 부서직업별 합계를 계산
```

<hr/>
### 윈도우 함수(Window Function)

- 행과 행 간의 관계를 정의하기 위한 함수
- 순위, 합계, 평균, 행 위치 등 조작 가능

<hr/>
### 테이블 파티션(Table Partition)

- 대용량 테이블을 여러 개의 데이터 파일에 분리해서 저장

- 각 파티션 별로 독립적으로 관리 가능

- 파티션은 논리적 관리 단위인 테이블 스페이스 간에 이동이 가능하다.

- 데이터를 조회할 때 데이터의 범위를 줄여서 성능이 향상된다.

  

- 종류

  - Rnage Partition : 값의 범위를 기준으로 여러 파티션으로 데이터 분할

  - List Partition : 특정 값을  기준으로 분할 하는 방법

  - Hash Partition : 시스템 내부적으로 해시함수를 사용해서 분할

    

- 파티션 인덱스

  - Global Index : 여러 개의 파티션에서 하나의 인덱스를 사용
  - Local Index :  해당 파티션 별로 각자의 인덱스를 사용
  - Prefixed Index : 파티션 키와 인덱스 키가 동일하다.
  - Non Prefixed Index : 파티션 키와 인덱스 키가 다르다.

<hr/><hr/>
# SQL 최적화의 원리

<hr/><hr/>
### 옵티마이저(Oprimizer)

- SQL의 실행 계획을 수립하고 SQL을 실행하는 관리 시스템 소프트웨어

- 옵티마이정에 따라 성능이 달라진다.

- 여러 실행 계획 중에서 최저비용인 계획을 선정하여 실행

- HINT를 사용하여 실행 계획 변경 을 요청할 수 있다.

- 옵티마이저는 15개의 우선순위를 기준으로 실행계획을 수립한다.

  

- 실행 순서

  개발자 SQL -> 파싱 -> 통계 -> 실행계획 -> SQL 실행

  

- 엔진 종류

  - Query Transformer : SQL문을 효울적으로 실행하기 위해서 옵티마이저가 변환한다.(실행결과는 동일)

  - Esimator : 통계정보를 통해 실행비용 계산

  - Plan Generator : SQL을 실행할 계획을 수립

    

- 비용 기반 옵티마이저 

  - 오브젝트 통계 및 시스템 통계를 사용하여 총비용을 계산하여 적은쪽으로 계획 수립

  - 총비용 = 소요시간, 자원의 사용량

    

- 인덱스

  - 데이터를 빠르게 검색할 수 있는 방법을 제공

  - 인덱스키로 정렬되어 있기 때문에 원하는 정보를 빠르게 조회

  - 기본키는 자동으로 인덱스가 생성 됨

  - root block, branch block, leaf block으로 구분되며 leaf block은 double linked list로 되어 있음

    

  - 인덱스 스캔

    - 인덱스 유일 스캔(index unique scan) : 인덱스의 키 값이 중복되지 않은 경우 사용하면 발생

    - 인덱스 범위 스캔(index range scan) : select문에서 where문을 사용할 경우 발생, 양이 적은경우 full scan

    - 인덱스 전체 스캔(index full scan) : 인덱스가 많은 경우 전부 스캔

      

  - 옵티마이저 조인

    - nested loop join 

      - 하나의 테이블에서 데이터를 먼저 찾고 그다음 테이블을 조인하는 방식
      - random access가 가장 많이 발생

    - sort merge join 

      - sort area라는 메모리 공간에 모두 로딩한 후 sort 수행
      - 양이 많은경우 임시 영역에서 수행하며, 임시영역은 디스크에 있어 매우 느림
      - sort를 가장 많이 발생

    - hash  join 

      - 두 개의 테이블 중 작은 테이블을 hash메모리에 로딩하고 두 개의 테이블의 조인 키를 사용하여 

        해시 테이블 생성

      - 메모리를 많이 사용하며, cpu연산을 많이 사용

<hr/

# 오답 노트

- 3층 스키마 : 외부 - 개념 - 내부
- 데이터 품질 저하 요소3 : 중복, 비유연성, 비일관성
- not in
- natural join 에서 사용된 열은 식별자를 가질 수 없음 
- nologging : insert에서만 사용가능
- join 시 어느 부분에서 중복을 제어하는지에 따라 성능이 달라짐
- like %aaa% 시 정상적인 인덱스 레인지 스캔 불가
- 주식별자 도출기준  : 자주 사용되는 속성 사용, 이름으로 기술되는것 사용, 너무 많은 속성이 포함되지 않도록 함
- 정규화, 반정규화
- 슈퍼타입, 서브타입
- 특정 = 리스트, 범위 =  레인지
- 하나의 행을 스캔하는 경우 table full scan이 효율적
- nested loop
  1. 선행 테이블 첫번째 행 찾기
  2. 선행 테이블 조인 키를 가지고 후행 테이블 조인 키가 존재하는지 찾으러 가서 조인 시도
  3. 후행테이블 인덱스에 선행 테이블의 조인 키가 존재하는지 확인
  4. 인덱스에서 추출한 레코드 식별자를 이용하여 후행 테이블 엑세스
- 응답 시간 =  cpu + Queue
- 파싱
  - 소프트 파싱 : 파싱 정보를 저장하는 라이브러리에서 파싱 정보를 찾을 수 있는 경우
  - 하드 파싱 : 파싱 정보를 저장하는 라이브러리에서 파싱 정보를 찾을 수 없는 경우
- I/O
  - 물리적 IO : 메모리에 데이터가 없는 경우 실행
  - 논리적 IO : 메모리에 데이터가 있으면 실행
- 다이나믹 sql : string변수에 sql문을 문자열 형태로 실행, 꼭 하드파싱을 반복하지는 않음
- 카디널리티 = 선택도 * 전체 레코드 수
- view merging =  옵티마이저가 최적화를 위해 sql문을 자동으로 가공하는 작업
  - 집합연산자, rownum,connect by, 집계함수, 분석함수 사용시 불가능
- mius = expect















