## [Database SQL] JOIN

##### 조인이란?

> 두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법

테이블을 연결하려면, 적어도 하나의 칼럼을 서로 공유하고 있어야 하므로 이를 이용하여 데이터 검색에 활용한다.

<br>

#### JOIN 종류

---

- INNER JOIN
- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

<br>

<br>

- #### INNER JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile9.uf.tistory.com%2Fimage%2F99799F3E5A8148D7036659">

  교집합으로, 기준 테이블과 join 테이블의 중복된 값을 보여준다.

  ```sql
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A
  INNER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
  ```

  <br>

- #### LEFT OUTER JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile6.uf.tistory.com%2Fimage%2F997E7F415A81490507F027">

  기준테이블값과 조인테이블과 중복된 값을 보여준다.

  왼쪽테이블 기준으로 JOIN을 한다고 생각하면 편하다.

  ```SQL
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A
  LEFT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
  ```

  <br>

- #### RIGHT OUTER JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F9984CE355A8149180ABD1D">

  LEFT OUTER JOIN과는 반대로 오른쪽 테이블 기준으로 JOIN하는 것이다.

  ```SQL
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A
  RIGHT OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
  ```

  <br>

- #### FULL OUTER JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile24.uf.tistory.com%2Fimage%2F99195F345A8149391BE0C3">

  합집합을 말한다. A와 B 테이블의 모든 데이터가 검색된다.

  ```sql
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A
  FULL OUTER JOIN JOIN_TABLE B ON A.NO_EMP = B.NO_EMP
  ```

  <br>

- #### CROSS JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F993F4E445A8A2D281AC66B">

  모든 경우의 수를 전부 표현해주는 방식이다.

  A가 3개, B가 4개면 총 3*4 = 12개의 데이터가 검색된다.

  ```sql
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A
  CROSS JOIN JOIN_TABLE B
  ```

  <br>

- #### SELF JOIN

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile25.uf.tistory.com%2Fimage%2F99341D335A8A363D0614E8">

  자기자신과 자기자신을 조인하는 것이다.

  하나의 테이블을 여러번 복사해서 조인한다고 생각하면 편하다.

  자신이 갖고 있는 칼럼을 다양하게 변형시켜 활용할 때 자주 사용한다.

  ``` sql
  SELECT
  A.NAME, B.AGE
  FROM EX_TABLE A, EX_TABLE B
  ```
<br>

### JOIN의 종류와 차이점을 설명하면?

> 조인은 두 개 이상의 테이블이나 DB를 연결하여 데이터를 검색하기 위한 방법입니다.
> 
> 조인에는 inner join, left join, right join, outer join, cross join, self join이 있습니다.
>
> 차이점은 기준 테이블과 조인 테이블이 있을 때를 기준으로,
>
> 두 테이블이 동시에 갖는 데이터는 inner join,
>
> 기준 테이블이 갖는 데이터는 left join,
>
> 조인 테이블이 갖는 데이터는 right join,
>
> 테이블이 갖는 모든 데이터는 outer join,
>
> 모든 조합을 반환해 경우의 수를 계산하기 위해서는 cross join,
>
> 동일한 테이블에서 연관 데이터를 찾기 위해서는 self join의 방식을 쓴다는 것이 차이점입니다.
>
<br>

### JOIN 시에 N+1 문제에 대해서 알고 있는가?

> `N+1` 문제는 **하나의 메인 쿼리(1)를 실행한 후, 관련된 데이터 N개에 대해 추가적으로 N개의 쿼리를 실행하게 되는 상황**을 말합니다.
> 
> 
> 예를 들어, **게시물과 댓글** 데이터가 있다고 가정해 보겠습니다.
> 
> - **1개의 쿼리**로 게시물 목록을 가져온 후, 각 게시물에 달린 **댓글을 조회하기 위해 N개의 쿼리**가 개별적으로 실행되는 경우입니다.
> - 즉, 게시물 목록을 가져오는 `SELECT * FROM posts` 쿼리를 실행한 뒤, 각각의 게시물에 대해 `SELECT * FROM comments WHERE post_id = ?` 쿼리가 N번 실행됩니다.
> - 게시물이 10개 있다면, 총 11개의 쿼리(1 + 10)가 실행됩니다.
> 
> 이 문제는 주로 ORM에서 **Lazy Loading(지연 로딩)** 방식을 사용할 때 발생하는 것으로 알고 있습니다.
> 
> **지연 로딩**은 외래키를 통해 연관된 엔터티를 필요로 할 때에 비로소 가져오는 방식으로 DB에 부하를 최소화 하기 위한 방식입니다.
>

<br>

### JOIN과 Subquery의 차이점에 대해서 설명해라

> JOIN은 여러 테이블의 데이터를 한 번에 가져와 결합하는 반면, 서브쿼리는 하나의 쿼리 안에 다른 쿼리를 포함하여 특정 조건에 맞는 데이터를 추출합니다.
> 
> - 서브쿼리는 가독성 면에서 뛰어남
> - 조인의 경우 DB 엔진이 내부적으로 최적화하여 데이터를 한 번에 가져오기 때문에 성능 면에서 뛰어남
  

<br>

<br>

##### [참고]

[링크](<https://coding-factory.tistory.com/87>)
