# MySQL Database Administration Statements

## Account Manangement Statements

### [`GRANT`](https://dev.mysql.com/doc/refman/5.7/en/grant.html)

사용자에게 권한을 부여할 수 있는 명령.

```sql
-- 기본적인 쿼리 구조
GRANT {priv_type} ON {priv_level} TO {user}

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
GRANT SELECT, INSERT, UPDATE ON service.* TO 'admin'@'10.21.64.5';
```
