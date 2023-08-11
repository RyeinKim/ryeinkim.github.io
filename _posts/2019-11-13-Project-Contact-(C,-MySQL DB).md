---
layout: post
title: "Project: Contact (C, MySQL DB)"
---

전화번호부 프로젝트를 시작하고 난 뒤 파일 입출력과 모듈화 프로그래밍을 이용해서 프로젝트를
진행하고 있었다. 하지만 무언가 임팩트를 주기는 힘들었고 난이도도 조금 낮았기 때문에 3일정도
MySQL에 대해서 팀원들에게 가르쳐준 뒤 파일 입출력이 아닌 DB를 기반으로 하는 전화번호부를 제작하기로
하였습니다. 기존의 기능 중에서 추가와 목록보기만 유지를 하고 데이터를 저장하는 방식을 파일에서 DB로
교체 했다.
---
<br>

> MySQL

여러가지 DBMS 가 있지만 MySQL을 선택한 이유는 대중적으로 많이 사용하는 DBMS이고, 사용하기도 가장 편리하고
용이하다고 생각하여 MySQL로 진행하게 되었습니다. 짧은 기간에 배우고 사용해야 했기 때문에 이론적인 부분에
집중하여 가르치고 배웠다기 보다 이론은 간단히 하고 직접 사용하는 식으로 가르쳐 보았다.

<br>


> main.c

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <mysql.h>

#define DB_HOST "183.111.182.200"
#define DB_USER "reignking"
#define DB_PASS "djaak4250"
#define DB_NAME "reignking"
#define CHOP(x) x[strlen(x) - 1] = ' '

MYSQL *con;
MYSQL_RES *res;
MYSQL_ROW row;



void input_data(_contact *contact);
int insert(MYSQL *con, _contact *contact);
int print(MYSQL *con);

void main() {
	con = mysql_init(NULL);
	mysql_init(con);
	if (mysql_real_connect(con, DB_HOST, DB_USER, DB_PASS, DB_NAME, 3306, NULL, 0))
	{
		printf("success\n");
		mysql_set_character_set(con, "euckr");
		mysql_query(con, "set names euckr");
	}
	else printf("error");

	while (1)
	{
		int menu;
		_contact m1;

		printf("type : ");
		scanf("%d", &menu);

		switch (menu)
		{
			case 1:
			{
				input_data(&m1);
				insert(con, &m1);
				break;
			}
			case 2:
			{
				print(con);
				break;
			}
		}
		mysql_query(con, "set names euckr");
		mysql_close(con);
	}
}

void input_data(_contact *contact)
{
	printf("input name\n");
	scanf("%s", contact->name);
	printf("input  digit\n");
	scanf("%s", contact->digit);
	printf("input address\n");
	scanf("%s", contact->address);
}

int insert(MYSQL *con, _contact *contact)
{
	char query[1024];
	sprintf(query, "insert into test values ('%s', '%s', '%s')", contact->name, contact->digit, contact->address);
	mysql_query(con, query);
	res = mysql_store_result(con);
}

int print(MYSQL *con)
{
	int k;
	int field;
	mysql_query(con, "select * from test");
	res = mysql_store_result(con);
	field = mysql_num_fields(res);
	printf("%s %s %s ", "name", "digit", "address");
	while ((row = mysql_fetch_row(res))) {
		for (k = 0; k < field; k++)
			printf("%s\n", row[k]);
		printf("\n");
	}
}
```
