# SQL JOINs

테이블들 사이의 연관된 필드를 기준으로 테이블과 테이블을 합치는 것.

## JOIN의 종류

![SQL JOINs Cheatsheet](/images/sql-joins-cheatsheet.png)

### INNER JOIN

양쪽 모두 결합의 기준이되는 필드가 있고, 그 필드가 조건에 맞는 레코드를 보여줌.

### [LEFT, RIGHT] OUTER JOIN

한쪽을 기준으로 다른쪽 테이블의 데이터를 연결하는 방식. 다른쪽에 데이터가 없으면 NULL로 처리.

### CROSS JOIN

한쪽 테이블의 모든 레코드와 다른쪽 테이블의 모든 레코드를 연결. 연관된 필드가 없어도 무관.

### SELF JOIN

같은 테이블의 레코드끼리 서로 참조하는 경우 조인 가능.
