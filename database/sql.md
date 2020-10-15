## 관계 대수와 SQL

### 관계 대수란?
* 관계 대수는 어떻게 질의를 수행할 것인가 명시하는 절차적인 언어로, 기존의 릴레이션들로부터 연산자들을 적용해서 새로운 릴레이션을 생성한다. 산술 연산자 중 이항 연산자/ 단항 연산자가 있는 것처럼 관계 대수에서도 이항/단항 연산자가 있다. 

* 관계 대수연산자들은 필수적인 연산자와 질의를 편하게 하기 위해 있는 연산자들로 구분된다. 여러 연산자들 중 _설렉션과 프로젝션 연산자_만 단항 연산자(입력 릴레이션이 하나)이고 나머지는 모두 이항 연산자이다. 

| 필수인가 아닌가? | 연산자 | 표기법 | 역할 |
| ----------- | --- | --- | --- |
| 필수 | 설렉션(selection) | σ | 한 릴레이션에서 실렉션 조건에 맞는 투플들로 부분 집합 생성 <br> ex) Employee릴레이션의 3번 부서 사원을 고르시오 <br> σ부서번호 = 2(Employee)| 
| 필수 | 프로젝션(projection) | π | 한 릴레이션의 애트리뷰트들의 부분집합 <br> ex) Employee릴레이션의 모든 사원들의 직급을 구해라 <br> π직급(Employee) |
| 필수 | 합집합(union) | U | 합집합 호환이 되기 위해선 두 릴레이션의 각 칼럼의 수가 같고 각 칼럼에 대해서 도메인이 같아야한다. <br> 두 릴레이션 중 최소하나에는 속한 투플들로 구성된 릴레이션을 반환한다 |
| 필수 | 차집합(difference) | - | A - B 인 경우 A에는 속하지만 B에는 속하지 않는 투플들로 이루어진 릴레이션을 반환한다 |
| 필수 | 카티션 곱(Cartesian product) | X | A X B 인 경우 A릴레이션의 투플과 B 릴레이션의 투플의 모든 가능한 조합을 가진 릴레이션을 반환하는데, 효율적인 연산은 아니다. |
| 안 필수 | 교집합(intersection) | | 두 릴레이션에 모두 속한 투플들로 이루어진 릴레이션을 반환한다 |
| 안 필수 | 세타 조인(theta join) | | =, <= 등의 조인 조건을 만족하는 투플들로 이루어진 릴레이션을 반환한다. |
| 안 필수 | 동등 조인(equijoin) | | 세타 조인 중 = 조건을 이용한 것을 동등 조인이라고 한다 |
| 안 필수 | 자연 조인(natural join) | | 동등 조인한 결과 릴레이션 중, 조인을 위해 사용한 칼럼은 지운 것이 자연조인이다. |
| 안 필수 | 세미 조인(semijoin) | | |
| 안 필수 | 디비전(devision) | % | A % B 를 하면 A와 차수가 같고, 모든 B의 투플 ab(투플 a와 b를 결합한것)이 존재하는 투플로만 이루어진 릴레이션을 반환함 |

[추가적으로 읽어보면 좋을 자료](https://velog.io/@ieed0205/%EA%B4%80%EA%B3%84%EB%8C%80%EC%88%98-SQL-LEEToday)

### 관계 해석이란?
원하는 데이터만 명시하고 질의를 어떻게 수행할 것인지는 명시하지 않는 선언적인 언어

### SQL이란?
SQL은 비절차적인 언어로 자신이 원하는 바를 명시하지만 처리하는 방법은 명시하지 않는 언어이다. <br>
데이터 정의어: create, alter, drop <br>
데이터 조작어: select, insert, delete, update <br>
데이터 제어어로 구성된다. 

1. SELECT: 관계 대수의 설렉션과 다르게, 관계 대수의 설렉션, 프로젝션, 조인, 카티션 곱이 결합된 것이다. <br> 
관계 데이터베이스에서 정보를 검색하는 SQL문으로 
    ```
    SELECT 칼럼 선택
    FROM 테이블 이름
    WHERE 조건
    GROUP BY 속성
    HAVING 조건
    ORDER BY 정렬기준 속성 ASC/DESC
    ```
    의 형태로 사용한다. 
    * 집단 함수: select 질의의 결과로 나온 릴레이션에 적용되는 함수로 select절과 having 절에만 나타날 수 있다. 
    * 그룹화: group by를 사용하면 select절에는 각그룹마다 하나의 값을 갖는 애트리뷰트 집단함수, 그룹화에 사용된 애트리뷰트만 나타날 수 있다. 
      * ex: 모든 사원들에 대해서 사원들이 속한 부서별로 그룹화하고, 각 부서마다 부서번호, 평균 급여, 최대급여를 검색하라 
      ```
      SELECT DNO, AVG(SALARY), MAX(SALARY)
      FROM EMPLOYEE
      GROUP BY DNO;
      ```

    * Having 절: Having절을 이용해서 각 그룹이 만족해야하는 조건을 명시한다. Having 절에 쓰인 속성은 반드시 GROUP BY절에 나타나거나, 집단함수가 적용되는 값이어야한다. 
      * ex: 모든 사원들에 대해서 사원들이 속한 부서번호별로 그룹화하고, 평균 급여가 a 이상인 부서에 대해서 부서번호, 평균 급여, 최대급여를 구하라.
      ```
        SELECT DNO, AVG(SALARY), MAX(SALARY)
        FROM EMPLOYEE
        GROUP BY DNO
        HAVING AVG(SALARY) > a
      ``` 

1. INSERT
    ```
    INSERT INTO 테이블 명
    VALUES 값
    ```
    릴레이션에 투플을 삽입하는 명령어로, 참조되는 릴레이션에 투플을 삽입할때는 참조 무결성 제약조건을 위배하지않으나, 참조하는 릴레이션에 삽입하는 경우 참조 무결성 제약조건을 위배할 수 도 있다. 
2. DELETE
    ```
    DELETE
    FROM 릴레이션
    WHERE 조건
    ```
    릴레이션에서 투플을 삭제하는 명령어로, 참조되는 릴레이션에서 투플을 삭제하는 경우 참조 무결성 제약조건이 위배될 수 있으나, 참조하는 릴레이션에서 투플을 삭제하면 참조 무결서 제약조건을 위배하지 않는다. 

3. UPDATE
    ```
    UPDATE 테이블 명
    SET 칼럼이름 = 변경하고 싶은값
    WHERE 조건
    ```
    한 릴레이션에 들어있는 투플들의 값을 수정하는 명령어로, 기본키/외래키 값이 수정되면 참조 무결성 제약조건을 위배할 수도 있다. 