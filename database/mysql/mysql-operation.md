# MySQL 운영

서비스 운영 중 만난 이슈들과 해결책

## [MySQL] 데이터가 많은 테이블에 컬럼이나 인덱스 추가 시

데이터가 많으면 테이블 수정 시 락 타임이 길어져 운영 중에는 문제가 발생할 수 있다. 이럴때는 `ALGORITHM`과 `LOCK`을 이용해 락을 걸지않고 컬럼이나 인덱스를 수정한다.

```sql
ALTER TABLE huge_table
ADD INDEX new_index (some_column), ALGORITHM = INPLACE, LOCK = NONE;
```

- `COPY` : MySQL 5.5 이하에서 사용된 방법. 데이터를 임시 테이블로 모두 복사후, rename하는 방식.
- `INPLACE` : MySQL 5.6 이상에 추가된 방법. 데이터를 바로 변경하되, 변경 작업 시 일어나는 DML작업들은 별도의 로그로 보관했다가 데이터 변경 마지막에 일괄 적용하는 방법. 온라인 변경 로그 공간을 충분히 확보해야 메모리 부족으로 작업이 실패하지 않음.
- `DEFAULT` : ALGORITHM를 명시하지 않은 것과 같음.
