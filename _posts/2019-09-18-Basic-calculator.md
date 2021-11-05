---
layout: post
title: "Basic Calculator (C, Modular Programming)"
---





Basic Calculator (C, Modular Programming)

---

원광대학교 중앙동아리 학술분과 소속 ENIAC 에서 진행한 팀 스터디 입니다.

C언어 스터디 팀을 구성하고 팀장을 맡아 팀원들에게 C언어를 가르쳐 주었습니다. 

<br>

> Modular Programming

C언어에 관련된 기본적인 내용은 1학년 커리큘럼에 포함되어 있습니다.

하지만 기본만 가르쳐줄 뿐 응용하는 것은 개개인의 몫이기 때문에 무엇을 가르쳐줄까 고민하던 중

모듈화 프로그래밍을 가르쳐주기로 결정했고 계산기 코드를 모듈화 하는 실습을 진행했습니다.

<br>

> calc.h

```c
#ifndef 

typedef struct _CALC_DATA { 
	int num1;
	int num2;
	char symbol;
	int result;
} CALC_DATA;

#endif

void add(CALC_DATA *data);
void sub(CALC_DATA *data); 
void print(CALC_DATA *data);
```

<br>

> func_calc.h

```c
#include "calc.h"

void add(CALC_DATA *data)
{
	data->symbol = '+';
	data->result = data->num1 + data->num2;
}

void sub(CALC_DATA *data)
{
	data->symbol = '-';
	data->result = data->num1 - data->num2;
}

void print(CALC_DATA *data)
{
	printf("%d %c %d = %d\n",
		data->num1,
		data->num2,
		data->symbol,
		data->result
	);
}
```

<br>

> main.c

```c
#include "func_calc.h"

int main()
{
	CALC_DATA data1;
	data1.num1 = 10;
	data1.num2 = 20;

	add(&data1);
	print(&data1);

	CALC_DATA data2;
	data2.num1 = 40;
	data2.num2 = 15;

	sub(&data2);
	print(&data2);

	return 0;
}
```

