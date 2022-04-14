---
layout: post
title: "Data Structure #1 - Sequential list"
---



Data Structure

---

학교에서 자료구조를 배우고 있습니다.

이전에도 배웠던 자료구조이지만 기록을 남겨두면 더 좋을 것 같아 학교에서 배우는 내용을 토대로

소스 파일을 한개씩 남겨보려 합니다.

순차 자료구조에 대해서 배운 후 실습을 진행했는데 다항식의 연산을 구현해내는 프로그램을

개발하는 실습을 진행 하였습니다.

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

순차 자료구조를 이용한 덧셈 예제인데 이를 이용해서 다항식의 곱셈을 제작하는 실습을

진행 하였습니다. 다항식의 곱셈에서는 덧셈과는 다르게 for문 사용해서 지수끼리의 덧셈을 진행하고 곱셈을 진행하는 방식으로 실습 하였습니다.

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
