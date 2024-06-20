---
layout: post
title: Mysql, MariaDB import, export
subtitle: import,export
tags: [mysqldump, Mysql, MariaDB]
comments: true
---

### At Glance
Mysql, MariaDB의 데이터베이스, 특정 테이블을 import, export 할 수 있다.

* export 
  
```sql
## Database export
mysqldump -uroot -p 데이터베이스명 > 데이터베이스_DUMP.sql

## Table export
mysqldump -uroot -p 데이터베이스명 테이블명 > 데이터베이스_DUMP.sql

## Encoding 옵션 적용
mysqldump -uroot -p --default-character-set=utf8 데이터베이스명 > 데이터베이스_DUMP.sql
```

<br>


* import
  
```sql
## Database import
mysql -uroot -p 데이터베이스명 < 데이터베이스_DUMP.sql

## Table import
mysql -uroot -p 데이터베이스명 < 데이터베이스_DUMP.sql

## Encoding 옵션 적용
mysql -uroot -p --default-character-set=utf8 데이터베이스명 < 데이터베이스_DUMP.sql
```

<br>
