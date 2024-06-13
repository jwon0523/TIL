# 원형 연결 리스트(circular linked list)
원형 연결 리스트는 마지막 노드가 첫 번째 노드를 가리키는 리스트이다. 즉, 마지막 노드의 링크가 NULL이 아닌 첫 번째 노드 주소를 가리키는 리스트이다.

## 장점

- 하나의 노드에서 다른 노드로의 접근이 가능
- 노드의 삽입과 삭제가 단순 연결리스트보다 용이함
- 단순 연결 리스트처럼 head와 tail 포인터 변수를 각각 두지 않고, 하나의 포인터 변수만 있어도 머리 또는 꼬리에 노드를 간단히 추가할 수 있다.
→ 즉, 원칙적으로 head 포인터만 있으면 됨.

# 원형 리스트의 삽입

## 첫번째에 삽입

![image](https://github.com/jwon0523/TIL/assets/50106190/09a560e8-d73f-4c7f-b466-d2c4ab5eab46)


먼저 새로운 노드가 첫 번째 노드를 가리키게 하고, 마지막 노드가 새 노드를 가리키게 하면 된다.

```cpp
ListNode* insert_first(ListNode* head, element data) {
	ListNode* node = (ListNode*)malloc(sizeof(ListNode));
	node->data = data;
	if (head == NULL) {
		head = node;
		node->link = head; // 자기 자신을 가리킴
	}
	else {
		node->link = head->link;
		head->link = node;
	}
	return head;
}
```

## 마지막에 삽입

원형 연결리스트는 원형이기 때문에 어디가 처음이고 끝인지는 불분명하다. 따라서 위 코드에서 한 문장만 추가하면 된다.

![image](https://github.com/jwon0523/TIL/assets/50106190/dfc1ac88-61b5-4e2e-99fb-3762fdddd7ef)


```cpp
ListNode* insert_last(ListNode* head, element data)
{
	ListNode* node = (ListNode*)malloc(sizeof(ListNode));
	node->data = data;
	if (head == NULL) {
		head = node;
		node->link = head;
	}
	else {
		node->link = head->link;
		head->link = node;
		head = node; // head가 새로운 노드를 가리킴
	}
	return head;
}
```

## 원형 연결 리스트의 출력

```cpp
void print_list(ListNode* head) {
	ListNode* p;
	if (head == NULL) return;
	p = head->link; // head는 마지막 노드이기 때문에 첫 노도를 p에 할당
	do {
		printf("%d->", p->data);
		p = p->link;
	} while (p != head->link); // p가 첫 노드를 가리키면 반복문 종료
}

```

가장 먼저 p에 head→link를 할당해 줬는데 head는 마지막 노를 가리키고 있기 때문에 head가 가리키고 있는 노드를 할당해 주는 것이다.

반복문의 경우 do-while로 진행해 줬는데, while로 반복문을 돌게 되면 다음 노드로 이동되기도 전에 p와 head→link가 첫 번째 노드를 가리키면서 반복문을 탈출하기 때문이다.

![image](https://github.com/jwon0523/TIL/assets/50106190/8d47ea65-ff9e-46e7-8708-76277d2073a7)

## 전체 코드

```cpp
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct ListNode {
	element data;
	struct ListNode* link;
} ListNode;

void print_list(ListNode* head) {
	ListNode* p;
	if (head == NULL) return;
	p = head->link; // head는 마지막 노드이기 때문에 첫 노도를 p에 할당
	do {
		printf("%d->", p->data);
		p = p->link;
	} while (p != head->link);
}

ListNode* insert_first(ListNode* head, element data) {
	ListNode* node = (ListNode*)malloc(sizeof(ListNode));
	node->data = data;
	if (head == NULL) {
		head = node;
		node->link = head;
	}
	else {
		node->link = head->link;
		head->link = node;
	}
	return head;
}

ListNode* insert_last(ListNode* head, element data) {
	ListNode* node = (ListNode*)malloc(sizeof(ListNode));
	node->data = data;
	if (head == NULL) {
		head = node;
		node->link = head;
	}
	else {
		node->link = head->link;
		head->link = node;
		head = node;
	}
	return head;
}

int main() {
	ListNode* head = NULL;

	head = insert_last(head, 20);
	head = insert_last(head, 30);
	head = insert_last(head, 40);
	head = insert_first(head, 10);
	print_list(head);
	return 0;
}
```

> 출처   
C언어로 쉽게 풀어쓴 자료구조 - 천인구
>
