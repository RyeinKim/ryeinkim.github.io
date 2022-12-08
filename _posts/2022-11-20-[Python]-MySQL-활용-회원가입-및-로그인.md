---
layout: post
title: "MySQL을 연동한 회원가입/로그인 시스템"
---

Python

---

데이터베이스 서버를 구축 후 파이썬을 이용해 회원가입, 로그인 코드를 만들고 연동해보았습니다.

<br>

> sql_module.py

```python
import pymysql

conn = pymysql.connect(
    host='localhost',
    user='root',
    password='',
    db='pydb',
    charset='utf8'
)

cur = conn.cursor()
```
<br>
MySQL 서버에 접속하고, 설정을 하는 부분 입니다.
<br>
> check.py

```python
import sql_module as sqlm

sql = "SELECT userid FROM usertable"
sqlm.cur.execute(sql)

result = sqlm.cur.fetchall()

for i in result:
    print(i[0])
```
<br>
sql_module 을 import 하고 계정 정보를 체크하는 부분 입니다.
<br>
> register.py

```python
import sql_module as sqlm
import numpy

checker = False

userid = input("userID: ")
userpw = input("userPW: ")

sql = "SELECT userid FROM usertable"
sqlm.cur.execute(sql)

result = sqlm.cur.fetchall()
a_result = numpy.array(result)

for i in result:
    print(i)
    if userid == i[0]:
        print("same userId already in data")
        checker = True
        break    
 
if checker == False:
    sql = "insert into usertable values (%s, %s)"
    val = (userid, userpw)
    sqlm.cur.execute(sql, val)
    sqlm.conn.commit()
```
<br>
회원가입을 하는 부분입니다. 중복 ID 를 걸러내는 부분까지 포함 하였습니다.
<br>