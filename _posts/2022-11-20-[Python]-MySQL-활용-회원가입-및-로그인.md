---
layout: post
title: "[Python] MySQL을 연동한 회원가입/로그인 시스템"
---

친구의 과제를 도와주면서 아주 간단한 회원가입 프로그램을 리스트 변수를 활용해서 구현했었다.
<br>본인은 회원가입/로그인 프로그램을 만들 때 데이터베이스 연동하는 것을 무조건 필수라고 생각하기 때문에
<br>친구 과제가 끝났음에도 끝을 보고 싶어  데이터베이스 서버를 구축 후 파이썬을 이용해 회원가입, 로그인 코드를 만들고 연동해보았다.
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
MySQL 서버에 접속하고, 설정을 하는 모듈

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
sql_module 을 import 하고 계정 정보를 체크하는 부분

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
회원가입을 하는 부분
<br>중복 ID 를 걸러내는 부분까지 포함 해보았다.
---
그동안 C와 Java 를 주로 사용했었고 Python 도 경험해보고자 이런 프로그램을 간단하게 만들어 보았는데
<br>여러가지 언어를 해보면서 느끼는 점은 _**<u>컴퓨터 언어는 웬만하면 거의 비슷하다는 것</u>**_ 이다.