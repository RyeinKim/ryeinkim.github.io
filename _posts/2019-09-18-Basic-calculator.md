---
layout: post
title: "Basic Calculator (C, Modular Programming)"
---

Basic Calculator (C, Modular Programming)
---
---

원광대학교 중앙동아리 학술분과 소속 ENIAC 에서 진행한 스터디 프로젝트 입니다.<br>
1학년 대상 C언어 스터디 팀을 구성하고 팀장을 맡아 팀원들에게 C언어를 가르쳐 주는 시간을 가졌습니다.<br>
컴퓨터 학과라고는 하지만 1학년이고, 사실 C언어도 모르는 상태로 왔지만 개발이 해보고 싶은 학생들은
동아리에 들어오는 것이 일반적입니다. (학과 수업으로는 한계가 분명히 존재하기 때문..)<br>
그래서 C언어를 해보고자 하는 1학년 학생들을 모아 스터디 프로젝트 팀장을 맡아 C언어를 최대한 활용해보자라는
<br>목표를 갖고 한 학기간 스터디 프로젝트를 진행해보았습니다.

<br>

> Modular Programming

C언어에 관련된 기본적인 내용(ex. 변수, 조건문, 반복문 등)은 1학년 커리큘럼에 포함되어 있습니다.<br>
하지만 기본만 가르쳐줄 뿐 응용하는 것은 개개인의 몫이기 때문에 무엇을 가르쳐줄지, 학과 강의에서는 가르쳐 주지 않지만 좋은 스킬이 어떤 것이 있는지 고민하고 있었습니다.
<br>팀 내에서 고민과 미팅을 한 후에 관심을 두고 있던 분야인 모듈화 프로그래밍을 가르쳐주기로 결정했고,
<br>저도 혼자 배워봄과 동시에 제가 배운 지식을 토대로 가르쳐 주며 계산기 코드를 모듈화 하는 실습을 진행했습니다.
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

---
사실 현재시점에서 보면 모듈화 프로그래밍이라고 할 수는 있겠으나 정석의 방법은 아니라고 생각되는 방식으로
<br>구현을 했다라는 생각을 한다. 그때 당시에는 헤더파일을 통해 기능을 모듈화 하고 main.c 에서 해당 헤더들을 포함하는 식으로 
구현을 했는데 다시금 되돌아보았을 때 더 좋은 방법들이 있었지 않았을까 라는 생각을 한다.<br><br>

>1. 헤더파일 & 소스파일을 사용한 모듈화

제가 구현했던 방식은 헤더파일안에 모듈의 인터페이스와 함수 구현부분을 모두 합쳐 두었는데, 더 이상적인 방법은 헤더파일과 소스파일을 나누고 메인프로그램에서 사용하는 것이 더 이상적이라고 생각한다.
<br>_Ex) 헤더 파일 안에는 함수와 구조체를 선언해두고 소스파일에는 헤더파일에 정의한 함수들의 실제 구현부분을 작성해서 
메인 프로그램에서 모듈의 헤더파일을 포함시키는 방식_
<br>
<br>
>2. 동적 라이브러리를 사용

이 방식은 프로그램을 실행할 때 모듈을 동적으로 로드하는 방식이다.
1. 모듈 작성
2. 동적 라이브러리 생성 (DLL or .so)
3. 메인 프로그램

이 방식은 dlopen() or LoadLibrary() 함수 등을 사용해 동적 라이브러리를 로드하고, dlsym() or GetProcAddress() 함수를 사용해 함수를 호출하는 방식으로 이루어진다.

---

동적 라이브러리를 사용하는 모듈화 방식을 나중에라도 꼭 한번 해봐야겠다.