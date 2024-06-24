# 연결 리스트를 이용한 스택

배열로 구현한 스택과 외부 인터페이스는 완전히 동일하나 내부 구현을 배열이 아닌 연결 리스트로 하는 것이다. 연결 리스트로 스택을 구현하면 다음과 같은 장점이 있다.

## 장점과 단점

- 장점: 동적 메모리 할당만 할 수 있다면 크기가 제한되지 않는다.
- 단점: 동적 메모리 해제를 해야 하기 때문에 삽입이나 삭제 시간은 좀 더 오래 걸린다.

# 스택 구현

```cpp
typedef int element;
typedef struct StackNode {
	element data;
	struct StackNode *link;
} StackNode;

typedef struct {
	StackNode *top;
} LinkedStackType;
```

기본적인 노드 구조체는 단순 연결 리스트의 노드와 동일하다. 다만, LinkedStackType이라는 구조체를 새롭게 정의하여 top이 노드를 가리킬 수 있도록 하였다.

## 삽입 연산

연결 리스트를 활용한 스택은 개념적으로 단순 연결 리스트에서 맨 앞에 데이터를 삽입하는 것과 동일하다. 헤드 포인터가 top으로 바뀐 것 외에는 별 차이점이 없다.

삽입 연산에서는 먼저 동적 할당으로 노드를 생성하고, 이 노도를 첫 번째 노드로 삽입한다.

![image](https://github.com/jwon0523/TIL/assets/50106190/c299af55-0a93-4c99-9e17-508d2e48beed)


```cpp
void push(LinkedStackType *s, element item)
{
	StackNode *temp = (StackNode *)malloc(sizeof(StackNode));
	temp->data = item;
	temp->link = s->top;
	s->top = temp;
}
```

## 삭제 연산

삭제 연산에서는 top의 값을 top→link로 바꾸고 기존의 top이 가리키는 노드를 메모리 해제 해주면 된다.

![image](https://github.com/jwon0523/TIL/assets/50106190/c3acfc8d-7f31-4e0e-a139-7d949ebec02b)

스택의 공백 상태는 단순 연결 리스트와 동일하게 top 포인터가 NULL인 경우이다. 그리고 포화 상태는 동적 할당만 된다면 노드를 생성할 수 있기 때문에 없다고 생각해도 된다.

```cpp
element pop(LinkedStackType *s)
{
	if (is_empty(s)) {
		fprintf(stderr, "스택이 비어있음\n");
		exit(1);
	}
	else {
		StackNode *temp = s->top;
		int data = temp->data;
		s->top = s->top->link;
		free(temp);
		return data;
	}
}
```

## 전체 코드

```cpp
#include <stdio.h>
#include <stdlib.h>

typedef int element;
typedef struct StackNode {
	element data;
	struct StackNode* link;
} StackNode;

typedef struct {
	StackNode* top;
} LinkedStackType;

void init(LinkedStackType* s) {
	s->top = NULL;
}

int is_empty(LinkedStackType* s) {
	return (s->top == NULL);
}

int is_full(LinkedStackType* s) {
	return 0;
}

void push(LinkedStackType* s, element item) {
	StackNode* temp = (StackNode*)malloc(sizeof(StackNode));
	temp->data = item;
	temp->link = s->top;
	s->top = temp;
}

void print_stack(LinkedStackType* s) {
	for (StackNode* p = s->top; p != NULL; p = p->link)
		printf("%d->", p->data);
	printf("NULL \n");
}

element pop(LinkedStackType* s) {
	if (is_empty(s)) {
		fprintf(stderr, "Empty Stack\n");
		exit(1);
	}
	else {
		StackNode* temp = s->top;
		int data = temp->data;
		s->top = s->top->link;
		free(temp);
		return data;
	}
}

element peek(LinkedStackType* s) {
	if (is_empty(s)) {
		fprintf(stderr, "Empty Stack\n");
		exit(1);
	}
	
	return s->top->data;
}

int main() {
	LinkedStackType s;
	init(&s);
	push(&s, 1); print_stack(&s);
	push(&s, 2); print_stack(&s);
	push(&s, 3); print_stack(&s);
	pop(&s); print_stack(&s);
	pop(&s); print_stack(&s);
	pop(&s); print_stack(&s);

	return 0;
}
```

> 출처   
C언어로 쉽게 풀어쓴 자료구조 - 천인구
>
