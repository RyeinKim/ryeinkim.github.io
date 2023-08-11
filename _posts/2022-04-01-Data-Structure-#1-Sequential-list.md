---
layout: post
title: "Data Structure #1 - Sequential list"
---

학교에서 데이터구조를 배우고 있다.<br>
이전에도 잠깐 배웠던 데이터구조이지만 기록을 남겨두면 더 좋을 것 같아 학교에서 배우는 내용을 토대로
몇가지 소스 파일을 한개씩 남겨보려 한다.

순차 자료구조에 대해서 배운 후 실습을 진행했는데 다항식의 연산을 구현해내는 프로그램을
개발하는 실습을 진행 하였다.

<br>

> Sequential_list_SUM.c

```c
#include <stdio.h>
#define MAX(a,b) ((a>b)?a:b)
#define MAX_DEGREE 50

typedef struct {							
	int degree;							
	float coef[MAX_DEGREE];			
} polynomial;

polynomial sumPoly(polynomial, polynomial);
void printPoly(polynomial);

void main() {
	polynomial A = { 5, { 3, 1, 4, 3, 7, 5 } };			
	polynomial B = { 2, { 1, 0, 3 } };		
	
	polynomial C;
	C = mulPoly(A, B);		

	printf("\n A(x) = "); printPoly(A);			
	printf("\n B(x) = "); printPoly(B);			
	printf("\n C(x) = "); printPoly(C);			

	getchar();
}

polynomial sumPoly(polynomial A, polynomial B) {
	polynomial C;			
	int A_index = 0, B_index = 0, C_index = 0;
	int A_degree = A.degree, B_degree = B.degree;
	C.degree = MAX(A.degree, B.degree);

	while (A_index <= A.degree && B_index <= B.degree) {
		if (A_degree > B_degree) {											
			C.coef[C_index++] = A.coef[A_index++];
			A_degree--;														
		}
		else if (A_degree == B_degree) {
			C.coef[C_index++] = A.coef[A_index++] + B.coef[B_index++];		
			A_degree--;
			B_degree--;
		}
		else {
			C.coef[C_index++] = B.coef[B_index++];
			B_degree--;
		}
	}
	return C;		
} 

void printPoly(polynomial P) {
	int i, degree;
	degree = P.degree;

	for (i = 0; i <= P.degree; i++) {
		printf("%3.0fx^%d", P.coef[i], degree--);
		if (i < P.degree) printf(" +");
	}
	printf("\n");
}
```

순차 자료구조를 이용한 덧셈 예제다.<br>
이를 이용해서 다항식의 곱셈을 제작하는 실습을 진행 하였습니다.
<br>다항식의 곱셈에서는 덧셈과는 다르게 for문 사용해서 지수끼리의 덧셈을 진행하고
<br>곱셈을 진행하는 방식으로 실습 하였다.
<br>

> Sequential_list_PRODUCT.c

```c
#include <stdio.h>
#define MAX_DEGREE 50

typedef struct {						
	int degree;								
	float coef[MAX_DEGREE];			
} polynomial;

polynomial mulPoly(polynomial, polynomial, polynomial);
void printPoly(polynomial);

void main() {
	polynomial Ax = { 2, { 3, 1, 0 } };			
	polynomial Bx = { 3, { 1, 5, 1, 3 } };			
	polynomial Cx = { 2, { 2, 3, 1 } };				
	
	polynomial Dx;
	Dx = mulPoly(Ax, Bx, Cx);	

	printf("\n A(x) = "); printPoly(Ax);			
	printf("\n B(x) = "); printPoly(Bx);			
	printf("\n C(x) = "); printPoly(Cx);			
	printf("\n D(x) = "); printPoly(Dx);			

	getchar();
}

polynomial mulPoly(polynomial Ax, polynomial Bx, polynomial Cx) {
	polynomial Dx;	

	for (int i = 0; i < Ax.degree + Bx.degree + Cx.degree + 1; i++) {		
		Dx.coef[i] = 0;
	}
	Dx.degree = Ax.degree + Bx.degree + Cx.degree;							

	for (int i = 0; i < Ax.degree + 1; i++) {							
		for (int j = 0; j < Bx.degree + 1; j++) {
			for (int k = 0; k < Cx.degree + 1; k++) {
				Dx.coef[i + j + k] += Ax.coef[i] * Bx.coef[j] * Cx.coef[k];
			}
		}	
	}
	return Dx;		
}

void printPoly(polynomial P) {
	int i, degree;
	degree = P.degree;

	for (i = 0; i <= P.degree; i++) {
		printf("%3.0fx^%d", P.coef[i], degree--);
		if (i < P.degree) printf(" +");
	}
	printf("\n");
}
```
<br>
연결 자료구조를 C언어에서 구현을 하기 위해서는 구조체를 사용해 링크 값을 가져야한다.
<br>여기서 연결 자료구조란 데이터 요소와 다음 요소를 가리키는 포인터로 이루어진 구조로,
데이터의 동적 추가와 삭제가 효율적이며 메모리 공간을 유연하게 활용할 수 있는 자료구조를 뜻한다.
<br>

> Linked_List.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define _CRT_SECURE_NO_WARNINGS

//단순 연결 리스트의 노드 구조체
typedef struct ListNode {
	char data[4];
	struct ListNode* link;
} listNode;

//리스트의 시작을 나타내는 head 노드  구조체
typedef struct {
	listNode* head;
} linkedList_h;

//공백연결리스트를 생성
linkedList_h* createLinkedList_h(void) {
	linkedList_h* L;
	L = (linkedList_h*)malloc(sizeof(linkedList_h));
	L->head = NULL;
	return L;
}

int main() {
	linkedList_h* L;
	L = createLinkedList_h();
	printList(L); getchar();
}

void insertFirstNode(linkedList_h* L, char* x) {
	listNode *newNode;
	newNode = (listNode*)malloc(sizeof(listNode));
	strcpy(newNode->data, x);
	newNode->link = L->head;
	L->head = newNode;
}

void insertMiddleNode(linkedList_h* L, listNode* pre, char* x) {
	listNode *newNode;
	newNode = (listNode*)malloc(sizeof(listNode));
	strcpy(newNode->data, x);
	
	if (L == NULL) {
		newNode->link = NULL;
		L->head = newNode;
	}
	else if (pre == NULL) {
		L->head = newNode;
	}
	else {
		newNode->link = pre->link;
		pre->link = newNode;
	}
}

void insertLastNode(linkedList_h* L, char* x) {
	listNode* newNode;
	listNode* temp;
	newNode = (listNode*)malloc(sizeof(listNode));
	strcpy(newNode->data, x);
	newNode->link = NULL;
	if (L->head == NULL) {
		L->head == newNode;
		return;
	}
	temp = L->head;
	while (temp->link != NULL) temp = temp->link;
	temp->link = newNode;
}

void deleteNode(linkedList_h* L, char* x) {
	listNode *cur;
	listNode *old;
	if (L->head == NULL) return;

	cur = L->head;

	while (cur != NULL) {
		if (strcmp(cur->data, x) == 0) {
			printf("found %s\n", x);

			if (cur == L->head) {
				L->head = cur->link;
				free(cur);
				return;
			}
			else {
				old->link = cur->link;
				free(cur);
				return;
			}
		}
		old = cur;
		cur = cur->link;
	}
}
```