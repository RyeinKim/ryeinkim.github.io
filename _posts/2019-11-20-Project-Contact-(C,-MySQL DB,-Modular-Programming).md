---
layout: post
title: "Project: Contact (C, MySQL DB, Modular Programming)"
---



Project: Contact (C, MySQL DB)

---

저번에 만든 DB를 연동한 전화번호부는 프로젝트 결과물로 제출하기에는 턱없이 부족했습니다. 그래서 기존 파일 입출력 방식에서 사용한 모듈화 프로그래밍 방식을 추가했고, 기존에 있었지만 사라졌던 기능들인 삭제, 수정, 목록보기 기능을 다시 추가하였습니다.

<br>

> Project or Study

이번 프로젝트에서 가장 중요한 것은 결과물을 내야하는 것은 맞지만 처음 시작은 스터디를 목적으로 모인 팀이기 때문에 팀원들에게 C언어, 파일 입출력, 모듈화 프로그래밍, 데이터베이스(MySQL)를 가르쳤고 그것을 모두 적용할 수 있는 주제를 생각해서 전화번호부를 만들고자 하였습니다.

최종적으로 결과물을 낼 때는 모듈화 프로그래밍의 장점을 살리고자 개개인이 각각의 모듈을 만들어오면

제가 만들어둔 코어에 갖다 붙이는 식으로 프로그램을 완성 하였습니다. 처음 취지에 맞게 프로젝트를 잘 끝냈고 결과도 매우 좋았습니다. 좋은 경험을 할 수 있었던 것 같습니다.

<br>

> main.h

```c
#define _CRT_SECURE_NO_WARNINGS
#include <mysql.h>
#include <string.h>
#include <stdio.h>

#pragma once

#define DB_HOST "reignking.cafe24.com"
#define DB_USER "reignking"
#define DB_PASS "djaak4250"
#define DB_NAME "reignking"
#define CHOP(x) x[strlen(x) - 1] = ' '

typedef struct {
	char name[10];
	char digit[20];
	char address[256];
} _contact;

MYSQL_RES   *sql_result;
MYSQL_ROW   sql_row;
MYSQL *connection = NULL, conn;

static int query_stat;
static char query[255];
```

<br>

> start.c

```c
#include "main.h"
#include "input.h"
#include "start.h"
#include "print.h"
#include "search.h"
#include "delete.h"
#include "edit.h"

int main(void)
{
	while (1)
	{
		int m;
		_contact m1;
		mysql_start();

		printf("1. input data  2. print all  3. search  4. delete  5. edit  6. quit\n");
		printf("type: ");
		scanf("%d", &m);
		getchar();

		if (m == 6) { break; }
		switch (m)
		{
			case 1:
			{
				input_data(&m1);
				break;
			}
			case 2:
			{
				print(connection);
				break;
			}
			case 3:
			{
				search(connection);
				break;
			}
			case 4:
			{
				del_info(connection);
				break;
			}
			case 5:
			{
				edit_info(&m1);
				break;
			}
			default:
			{
				break;
			}
		}
		mysql_close(connection);
	}
}
```

이렇게 코어를 미리 짜서 각 팀원들이 볼 수 있도록 깃에 올려뒀고 각각 팀원들은 자신이 맡은 모듈에 대해서 해당 코어를 참고하여 각각 모듈을 제작하였습니다.

<br>

각 모듈 중 연락처 추가 기능을 만든 팀원A 입니다.

> input.h

```c
void input_data(_contact *contact);

static int query_stat;
static char query[255];

void input_data(_contact *contact)
{
	printf("이름: ");
	fgets(contact->name, 20, stdin);
	CHOP(contact->name);

	printf("번호: ");
	fgets(contact->digit, 20, stdin);
	CHOP(contact->digit);

	printf("주소: ");
	fgets(contact->address, 256, stdin);
	CHOP(contact->address);

	sprintf(query, "insert into test values "
		"('%s', '%s', '%s')",
		contact->name, contact->digit, contact->address);

	query_stat = mysql_query(connection, query);
	if (query_stat != 0)
	{
		fprintf(stderr, "Mysql query error : %s", mysql_error(&conn));
		return 1;
	}
}
```

<br>

각 모듈 중 연락처 삭제 기능을 만든 팀원B 입니다.

> delete.h

```c
void del_info(MYSQL *connection);

static char query[255];

void del_info(MYSQL *connection)
{
	char name[20];
	int field;
	printf("insert name : ");
	scanf("%s", name);
	sprintf(query, "delete from test where name = '%s'", name);
	mysql_query(connection, query);
}
```

<br>

각 모듈 중 연락서 수정 기능을 만든 팀원C 입니다.

>edit.h

```c
void edit_info(_contact *contact);

static int query_stat;
static char query[255];

void edit_info(_contact *contact)
{
	char old_name[20], old_num[30], old_address[256];
	int field, menu;
	printf("insert name : ");
	scanf("%s", old_name);
	sprintf(query, "select * from test where name = '%s'", old_name);
	mysql_query(connection, query);
	sql_result = mysql_store_result(connection);
	field = mysql_num_fields(sql_result);
	while ((sql_row = mysql_fetch_row(sql_result)))
	{
		for (int i = 0; i < field; i++)
			printf("%s\n", sql_row[i]);
		printf("\n");
	}

	printf("1. 이름바꾸기 2. 전화번호 바꾸기 3. 주소 바꾸기\n");
	scanf("%d", &menu);
	getchar();
	switch (menu)
	{
		case 1:
		{
			printf("바꿀이름: ");
			fgets(contact->name, 20, stdin);
			CHOP(contact->name);

			sprintf(query, "update test set name = '%s' where name = '%s'", contact->name, old_name);
			query_stat = mysql_query(connection, query);
			break;
		}
		case 2:
		{
			printf("바꿀 번호: ");
			fgets(contact->digit, 20, stdin);
			CHOP(contact->digit);

			sprintf(query, "update test set digit = '%s' where name = '%s'", contact->digit, old_name);
			query_stat = mysql_query(connection, query);
			break;
		}
		case 3:
		{

			printf("바꿀 주소: ");
			fgets(contact->address, 256, stdin);
			CHOP(contact->address);

			sprintf(query, "update test set address = '%s' where name = '%s'", contact->address, old_name);
			query_stat = mysql_query(connection, query);
			break;
		}
	}

	if (query_stat != 0)
	{
		fprintf(stderr, "Mysql query error : %s", mysql_error(&conn));
		return 1;
	}
}
```

<br>

각 모듈 중 연락처 검색 기능을 만든 팀원D 입니다.

> search.h

```c
void search(MYSQL *connection);

static char query[255];

void search(MYSQL *connection)
{
	char name[20];
	int field;
	printf("insert name : ");
	scanf("%s", name);
	sprintf(query, "select * from test where name = '%s'", name);
	mysql_query(connection, query);
	sql_result = mysql_store_result(connection);
	field = mysql_num_fields(sql_result);
	while ((sql_row = mysql_fetch_row(sql_result)))
	{
		for (int i = 0; i < field; i++)
			printf("%s\n", sql_row[i]);
		printf("\n");
	}
}
```

<br>

각 모듈 중 연락처 목록보기 기능을 만든 팀원F 입니다.

> print.h

```c
void print(MYSQL *connection);

void print(MYSQL *connection)
{
	int field;
	mysql_query(connection, "select * from test");
	sql_result = mysql_store_result(connection);
	field = mysql_num_fields(sql_result);
	while ((sql_row = mysql_fetch_row(sql_result)))
	{
		for (int i = 0; i < field; i++)
		{
			printf("%s\n", sql_row[i]);
		}
		printf("-------------------");
		printf("\n");
	}
}
```

<br>

이렇게 만들어진 모듈들을 모두 코어에 붙혀서 전화번호부를 완성하였습니다.

