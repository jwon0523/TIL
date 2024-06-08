# 추상 클래스

인터페이스를 설명하기에 앞서 간략하게 추상 클래스에 대해 설명하자면, 추상 클래스는 순수가상함수가 하나 이상 있는 클래스를 말한다.   
따라서 추상 클래스는 파생 클래스에 의해 오버라이딩 되어야 하는 함수이다. 자세한 내용은 해당 [포스트](https://velog.io/@jwlee010523/abstract-class)에서 확인할 수 있다.

# 인터페이스(Interface)

인터페이스는 구현이 없다. 즉, 인터페이스 클래스에는 가상 소멸자와 순수 가상함수 선언을 기본 클래스로 지정하는 것을 말한다.

```cpp
class Shpae {
public:
	virtual ~Shape();
	virtual void info() = 0;
	virtual void draw() = 0;
};
```

인터페이스는 가상 소멸자가 필수이다. 그래야만 기본 클래스와 파생 클래스의 소멸자가 모두 실행되기 때문이다.

## 인터페이스의 필요성

```cpp
#include <iostream>

using namespace std;

class AbstractA {
public:
	void Print() { cout << "AbstractA" << endl; };
};

class AbstractB {
public:
	void Print() { cout << "AbstractB" << endl; };
};

class Child : public AbstractA, AbstractB {
public:
};

int main()
{
	Child child;

	child.Print(); // Error

	return 0;
}
```

이처럼 두 개의 클래스를 다중 상속 받았을 때 함수명이 겹치면 둘 중 어느 함수를 실행해야 할지 몰라 오류가 발생한다.   
이러한 상황을 방지할 수 있는 것이 인터페이스이다.

```cpp
#include <iostream>

using namespace std;

class InterfaceA {
public:
	virtual void Print() = 0;
};

class InterfaceB {
public:
	virtual void Print() = 0;
};

class Child : public InterfaceA, InterfaceB {
public:
	void Print() override { cout << "Child" << endl; };
};

int main()
{
	Child child;

	child.Print();

	return 0;
}

// 출력
// Child
```

이렇듯 인터페이스 방식으로 정의할 경우 다중 상속으로 인해 생기는 문제를 원천봉쇄할 수 있다.

# 차이점

- 추상 클래스는 순수 가상 함수 외에 상태(data members) 또는 구현을 포함할 수 있지만, 인터페이스는 순수 가상 함수만 포함해야 한다.
- 즉, 인터페이스는 상태나 구현을 가질 수 없다.
- 인터페이스를 상속받은 클래스는 해당 인터페이스의 모든 순수 가상 함수를 구현해야 한다.
- 추상 클래스는 추상 메소드를 구현하지 않고 상속될 수 있다.(유도 클래스 역시 추상클래스)
- **인터페이스는 다중 상속될 수 있지만 추상 클래스는 그렇지 않을 수 있다. → 이게 핵심**
   
> 출처   
[https://velog.io/@hyongti/C추상클래스-vs.-인터페이스-클래스](https://velog.io/@hyongti/C%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-vs.-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%ED%81%B4%EB%9E%98%EC%8A%A4)   
[https://wisdom-coding.tistory.com/5 [인(仁)코딩:티스토리]](https://wisdom-coding.tistory.com/5)
>
