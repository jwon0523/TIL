# 이중 연결 리스트(Doubly linked list)
이중 연결 리스트는 한쪽으로만 노드를 연결하는 것이 아닌 양방향으로 연결한 리스트를 말한다.

![image](https://github.com/jwon0523/TIL/assets/50106190/2ffcee29-6b84-4af5-aaa7-10fa6304ab08)


실제 응용에서는 위 그림처럼 이중 연결리스트와 원형 연결리스트를 혼합한 형태가 많이 사용된다. 또한 헤드 노드(head node)라는 더미 노드가 추가되는 경우도 많다.

헤드 포인터와는 다른 개념인데, 헤드 포인터는 리스트의 첫 번째 노드를 가리키는 포인터이고, 헤드 노드는 데이터를 가지고 있지 않는 더미 노드를 추가하는 것이다. 헤드 노드가 존재했을 때 이점은 삽입, 삭제 알고리즘이 간편해진다는 것이다. 리스트가 공백일 경우에 헤드 노드는 자기 자신을 가리킨다.

![image](https://github.com/jwon0523/TIL/assets/50106190/b6ad1aab-41a8-4ecb-b984-a11cf5faf353)


## 노드 구조체의 정의

```cpp
typedef int element;
typedef struct DListNode {
	element data;
	struct DListNode* llink;
	struct DListNode* rlink;
} DListNode;
```

llink는 왼쪽 노드를 연결하고, rlink는 오른쪽 노드를 연결한다.

## 삽입 연산

![image](https://github.com/jwon0523/TIL/assets/50106190/d2a719cf-0d13-4085-90d4-9776fd86e4f5)


위 그림처럼, 새로 만들어진 노드의 링크를 먼저 바꾼다. 새 노드가 아닌 기존 노드 링크의 값을 바꾸게 된다면 주소를 잃어버리기 때문이다. 코드는 다음과 같다.

```cpp
void dinsert(DListNode *before, element data)
{
	DListNode *newnode = (DListNode *)malloc(sizeof(DListNode));
	newnode->data = data;
	newnode->llink = before; // 1번 과정
	newnode->rlink = before->rlink; // 2번 과정
	before->rlink->llink = newnode; // 3번 과정
	before->rlink = newnode; // 4번 과정
}
```

## 삭제 연산

![image](https://github.com/jwon0523/TIL/assets/50106190/78e77520-15f9-4962-9ebd-f53e518b63e6)


삭제할 노드의 연결을 모두 끊어준 후 삭제할 노드를 메모리 해제 해주면 된다. 

코드는 다음과 같다.

```cpp
void ddelete(DListNode* head, DListNode* removed)
{
	if (removed == head) return;
	removed->llink->rlink = removed->rlink;
	removed->rlink->llink = removed->llink;
	free(removed);
}
```

## 전체 코드

```cpp
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct DListNode {
	element data;
	struct DListNode* llink;
	struct DListNode* rlink;
} DListNode;

void init(DListNode* phead) {
	phead->llink = phead;
	phead->rlink = phead;
}

void print_dlist(DListNode* phead) {
	DListNode* p;
	for (p = phead->rlink; p != phead; p = p->rlink) {
		printf("<-| |%d| |-> ", p->data);
	}
	printf("\n");
}

void dinsert(DListNode* before, element data) {
	DListNode* newNode = (DListNode*)malloc(sizeof(DListNode));
	newNode->data = data;
	newNode->llink = before;
	newNode->rlink = before->rlink;
	before->rlink->llink = newNode;
	before->rlink = newNode;
}

void ddelte(DListNode* head, DListNode* removed) {
	if (removed == head) return;
	removed->llink->rlink = removed->rlink;
	removed->rlink->llink = removed->llink;
	free(removed);
}

int main() {
	DListNode* head = (DListNode*)malloc(sizeof(DListNode));
	init(head);
	printf("추가 단계\n");
	for (int i = 0; i < 5; i++) {
		dinsert(head, i);
		print_dlist(head);
	}
	printf("\n삭제 단계\n");
	for (int i = 0; i < 5; i++) {
		print_dlist(head);
		ddelte(head, head->rlink);
	}
	free(head);

	return 0;
}
```

> 출처   
C언어로 쉽게 풀어쓴 자료구조 - 천인구
>
