---
layout: post
title: "Project: Contact (C, Modular Programming)"
---



Project: Contact (C, Modular Programming)

---

저번 글에서 만들었던 파일 입출력을 이용한 전화번호부에 이전에 계산기 예제를 이용해 가르쳤던 모듈화 프로그래밍을 적용 하였습니다,

사실 이전에 계산기 예제를 이용해서 모듈화 프로그래밍을 가르쳤던 것도 프로젝트를 진행할 것을 염두에 두고 가르친 것이기 때문에 모듈화 프로그래밍을 적용했습니다.

<br>

> main.h

```c
#include <stdio.h>
#include <string.h>
#define _CRT_SECURE_NO_WARNINGS
#ifndef CONTACT_DATA

typedef struct _CONTACT_DATA {
	char name[20];
	char pnum[30];
	char address[1024];
	int menu;
} C_DATA;

#endif

void filesave(C_DATA *data);
void fileload();
void inputinfo();
void filesearch();
void filemodify();
```

<br>

>db.h

```c
#include "main.h"

void filesave(C_DATA *data)
{
	FILE *fp = fopen("contact.dll", "a");
	fprintf(fp, "%s %s %s\n", data->name, data->pnum, data->address);
	fclose(fp);
}

void fileload()
{
	char strTemp[255];
	char *pStr;
	int count = 0;

	C_DATA data2[200];
	FILE *fp = fopen("contact.dll", "r");
	if (fp != NULL)
	{
		while (!feof(fp))
		{
			fscanf(fp, "%s %s %[^\n]\n", data2[count].name, data2[count].pnum, data2[count].address);
			printf("%s %s %s\n", data2[count].name, data2[count].pnum, data2[count].address);
			count++;
		}
	}
	else {}
	fclose(fp);
}

void inputinfo()
{
	C_DATA data1;
	getchar();
	printf("Name: ");
	scanf("%[^\n]s", data1.name);

	printf("H.P: ");
	scanf("%s", data1.pnum);
	getchar();

	printf("Address: ");
	scanf("%[^\n]", data1.address);

	filesave(data1.name, data1.pnum, data1.address);
}

void filesearch()
{
	int count = 0;
	char type[20];

	C_DATA data2[200];
	FILE *fp = fopen("contact.dll", "r");
	while (!feof(fp))
	{
		fscanf(fp, "%s %s %[^\n]", data2[count].name, data2[count].pnum, data2[count].address);
		count++;
	}
	scanf("%s", type);
	for (int i = 0; i < count; i++)
	{
		if (strcmp(data2[i].name, type) == 0)
		{
			printf("%s %s %s\n", data2[i].name, data2[i].pnum, data2[i].address);
		}
	}
}

void filemodify()
{
	int count = 0;
	char type[20], chname[30], chpnum[30], chaddress[1024];

	C_DATA data2[200];
	FILE *fp = fopen("contact.dll", "r");
	FILE *fp2 = fopen("contact_ex.dat", "a");
	while (!feof(fp))
	{
		fscanf(fp, "%s %s %[^\n]", data2[count].name, data2[count].pnum, data2[count].address);
		count++;
	}
	scanf("%s", type);
	for (int i = 0; i < count; i++)
	{
		if (strcmp(data2[i].name, type) == 0)
		{
			printf("%s %s %s\n", data2[i].name, data2[i].pnum, data2[i].address);

			printf("input new name: ");
			scanf("%s", chname);
			strcpy(data2[i].name, chname);
			
			printf("input new H.P: ");
			scanf("%s", chpnum);
			strcpy(data2[i].pnum, chpnum);

			printf("input new address: ");
			scanf("%[^\n]", chaddress);
			strcpy(data2[i].pnum, chpnum);
		}
	}
	for (int i = 0; i <= count; i++)
	{
		fprintf(fp2, "%s %s %s\n", data2[i].name, data2[i].pnum, data2[i].address);
	}
	fclose(fp);
	fclose(fp2);
	remove("contact.dll");
	rename("contact_ex.dat", "contact.dll");
}
```

<br>

> main.c

```c
#include "db.h"

int main() 
{
	while (1)
	{
		int menu;
		printf("1. Add contact  2. View Contact  3. Search  4. Quit\n");
		scanf("%d", &menu);
		if (menu == 5) { break; }
		switch (menu)
		{
			case 1:
			{
				inputinfo();
				break;
			}
			case 2:
			{
				fileload();
				break;
			}
			case 3:
			{
				printf("이름 : ");
				filesearch();
				break;
			}
			case 4:
			{
				filemodify();
			}
		}
	}		
	return 0;
}
```

