---
layout: post
title: "Project: Contact (C, Files I/O)"
---

Project: Contact (C, Files I/O)
---
---

저번 글에 설명했던 것처럼 스터디 프로젝트 팀장을 맡고 있었는데, 
최종적인 결과물을 보여줄 수 있는 프로젝트를 한가지 진행해야 했다.
어떤 주제로 프로젝트를 기획해야 C언어를 잘 활용하고 의미가 있을까 생각하던 중
파일입출력을 사용해봐야겠다는 생각이 들었고 가장 무난한 전화번호부를 만들어보는게
괜찮다고 생각해 전화번호부 프로젝트를 시작했다.

<br>

> Files I/O

**파일입출력은 1학년 C언어 커리큘럼에서 아주 조금 다루기 때문에,<br>
수업을 들었다고 해도 구현을 해내기는 쉽지않다. <br>
그렇기 때문에 파일입출력에 대해서 먼저 공부를 하고 후에  전화번호부를 제작했다..**
<br>

> main.c

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>
#define MAX_LENGTH 2048

void filesave(char *a, char *b, char *c);
void fileload();
void filesearch(int num);

int main()
{
	while (1)
	{
		int menu, num1;
		printf("1. 등록  2. 조회  3. 종료  4. 검색\n");
		scanf("%d", &menu);

		switch (menu)
		{
			case 1:
			{
				int num;
				char name[25];
				char pnum[30];
				char address[1024];

				getchar();
				printf("이름: ");
				scanf("%[^\n]s", &name);

				printf("H.P: ");
				scanf("%s", &pnum);
				getchar();

				printf("주소: ");
				scanf("%[^\n]s", &address);

				filesave(name, pnum, address);
				break;
			}
			case 2:
			{
				fileload();
				break;
			}
			case 4:
			{
				printf("줄 수 검색 : ");
				scanf("%d", &num1);
				filesearch(num1);
				break;
			}
		}
	}
	return 0;
}

void filesave(char *a, char *b, char *c)
{
	FILE *fp = fopen("contact.dll", "a");
	fprintf(fp, "이름:\t%s\nH.P:\t%s\n주소:\t%s\n", a, b, c);
	fprintf(fp, "=========================\n");
	fclose(fp);
}

void fileload()
{
	char strTemp[255];
	char *pStr;

	FILE *fp = fopen("contact.dll", "r");
	if (fp != NULL)
	{
		while (!feof(fp))
		{
			pStr = fgets(strTemp, sizeof(strTemp), fp);
			printf("%s", strTemp);
		}
		fclose(fp);
	}
	else	{}
}

void filesearch(int num)
{
	char strTemp[255];
	char *pStr;
	char buffer[MAX_LENGTH];
	int count = 0;

	FILE *fp = fopen("contact.dll", "rt");
	if (fp != NULL)
	{
		while (fgets(buffer, MAX_LENGTH, fp) != NULL)
		{
			count++;
			if (count == num)
			{
				printf("%d번째 줄 : %s\n", count, buffer);
				break;
			}
		}
		pStr = fgets(strTemp, sizeof(strTemp), fp);
		printf("%s", strstr(fp, "김례인"));
	}
	else {}
	fclose(fp);
}
```
---
contact.dll 이라는 파일에 전화번호부 정보를 저장하고
* filesave
* fileload
* filesearch

함수를 정의하여 저장, 목록보기, 검색을 구현하였습니다.
포인터 변수의 경우는 문자열 및 데이터 크기가 다양한 데이터들을 처리해야하고, 함수간 데이터를 공유해야하기 때문에 사용했다..