---
layout: post
title: "Data Structure #2 - Linked list"
---



Data Structure

---

이전 수업에서는 순차 자료구조에 대해서 배웠고 이번에는 연결 자료구조에 대해서 배웠습니다.

연결 자료구조를 C언어에서 구현을 하기 위해서는 구조체를 사용해 링크 값을 가져야합니다.

소스 바로 적어 두겠습니다.

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