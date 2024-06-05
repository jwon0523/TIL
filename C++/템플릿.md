# 제네릭과 템플릿

## 제네릭(generic)

제네릭이란 자료형의 일반화로써 함수나 클래스를 일반화 시키고, 매개 변수 타입을 지정하여 틀에서 찍어 내듯이 함수나 클래스 코드를 생산하는 기법이다.

제네릭 타입은 어떤 명칭이든 상관 없지만, 하나의 제네릭 타입만 선언할 경우 관례적으로 T를 사용한다.

```cpp
template <class T>
```

만약 두 개 이상의 제네릭 타입을 가질 경우에는 여러개 선언해 주면 된다.

```cpp
template <class T1, class T2, class T3>
```

템플릿 선언부는 한 줄에 붙여 써도 상관 없다.

```cpp
template <class T> void myswap(T& a, T& b) { ... }
```

## 템플릿(template)

제네릭 타입을 이용해 함수나 클래스를 일반화하는 도구로써 함수나 클래스를 개별적으로 다시 작성하지 않아도 여러 자료 형으로 사용할 수 있도록 만들어 놓은 것으로, 일종의 틀이라고 볼 수 있다.

템플릿을 사용하는 이유는 기존의 함수 오버로딩으로는 모든 자료형을 미리 예측하여 구현할 수 없다는 단점이 있기 때문이다.

# 템플릿 함수

template 키워드를 이용하면 오버로딩 함수들을 일반화시킬 수 있다. 

```cpp
template <class T>
template <typename T>
```

템플릿 선언은 class로 지정하든 typename으로 지정하든 별 상관 없다.

![image](https://github.com/jwon0523/TIL/assets/50106190/f71e2ef4-17f9-4926-a52d-289ee67679a3)

## 템플릿으로부터 구체화

오버로딩된 함수들을 템플릿화하는 과정의 역과정을 구체화라고 하며, 이를 구체화된 함수라고 한다. 

컴파일러가 함수 호출시, 구체화를 통해 제제릭 함수로부터 구체적인 함수의 소스 코드를 만들어낸다.

![image](https://github.com/jwon0523/TIL/assets/50106190/6ea790e0-a8ca-45e5-ba19-cce69b6fc31c)

위 그림을 자세히 설명해 보면 다음과 같다.

1. 컴파일러는 myswap(a,b); 호출문을 컴파일 할 때 myswap() 함수를 찾고, 템플릿으로 선언된 myswap() 함수를 발견한다.
2. myswap(a,b); 함수 호출문에서 실인자 a,b가 모두 Int 타입이므로, 템플릿의 제네릭 타입 T에 int를 대입시켜 구체화된 버전의 myswap(int& a, int& b)로 만든다.
3. 구체화된 함수를 호출하도록 컴파일한다.

아래는 3개의 함수 호출에 대해 구체화시키는 과정이다.

![image](https://github.com/jwon0523/TIL/assets/50106190/452fa4f3-6b4a-474a-8f76-c74ffedc094c)


### 예제 코드

```cpp
#include <iostream>
using namespace std;

class Circle {
  int radius;

public:
  Circle(int radius = 1) : radius(radius) {}
  int getRadius() { return radius; }
};

template <class T>
void myswap(T& a, T& b) {
  T tmp;
  tmp = a;
  a = b;
  b = tmp;
}

int main() {
  int a = 4, b = 5;
  myswap(a,b);
  cout << "a=" << a << ", " << "b=" << b << endl;

  double c=0.3, d=12.5;
  myswap(c, d);
  cout << "c=" << c << ", " << "d=" << d << endl;
  
  Circle donut(5), pizza(20);
  myswap(donut, pizza);
  cout << "donut's radius: " << donut.getRadius() << ", ";
  cout << "pizza's radius: " << pizza.getRadius() << endl;

  return 0;
}

// 출력
// a=5, b=4
// c=12.5, d=0.3
// donut's radius: 20, pizza's radius: 5
```

## 구체화 오류

제네릭 타입에 구체적인 타입 지정시 일관되게 사용해야 한다.

```cpp
template <class T> void myswap(T& a, T& b);
```

```cpp
int s = 5;
double t = 3.5;
myswap(s, t); // 컴파일 오류. 구체화 불가능.
```

만약 여러개의 타입을 지정하고 싶다면, 제네릭을 여러개 선언해 주면 된다.

```cpp
#include <iostream>
using namespace std;

template <class T1, class T2> 
void mcopy(T1 src[], T2 dest[], int n) {
  for (int i = 0; i < n; i++)
    dest[i] = (T2)src[i];
}

int main() {
  int x[] = {1, 2, 3, 4, 5};
  double d[5];
  char c[5] = {'N', 'i', 'c', 'e', '!'}, e[5];

  mcopy(x, d, 5); // 서로 다른 타입
  mcopy(c, e, 5); // 동일한 타입

  for (int i = 0; i < 5; i++)
    cout << d[i] << ' ';
  cout << endl;
  for (int i = 0; i < 5; i++)
    cout << e[i] << ' ';
  cout << endl;

  return 0;
}

// 출력
// 1 2 3 4 5 
// N i c e ! 
```

mcopy(T1, T2, int) 함수는 서로 다른 타입을 받을 수 있고, 제네릭 타입이 T1과 T2로 다르더라도 둘 다 동일한 타입을 받을 수 있다.

## 장점과 단점

### 장점

- 함수 코드의 재사용을 가능하게 하여 소프트웨어의 생산성과 유연성 높임.

### 단점

- 초기에는 지원을 안 했기에, 컴파일러에 따라 템플릿 지원 안될 수 있어 포팅에 취약.
- 컴파일 오류 메시지가 빈약하여 디버깅 어려움.

디버깅의 어려움을 해소하려면 제네릭 프로그래밍을 사용하면 되는데, C++에서는 STL(Standart Template Libray)를 제공하고 있다.

## 오버로딩 함수와 템플릿 함수 우선순위

오버로딩 함수가 템플릿 함수보다 우선순위가 높다.

```cpp
#include <iostream>

using std::cout;
using std::endl;

template <class T> void print(T array[], int n) {
  for (int i = 0; i < n; i++)
    cout << array[i] << "\t";
  cout << endl;
}

void print(char array[], int n) {
  cout << "[오버로딩 함수]\n";
  for (int i = 0; i < n; i++)
    cout << (int)array[i] << '\t';

  cout << endl;
}

int main() {
  int x[] = {1, 2, 3, 4, 5};
  double d[5] = {1.1, 2.2, 3.3, 4.4, 5.5};

  print(x, 5);
  print(d, 5);

  char c[5] = {1, 2, 3, 4, 5};
  print(c, 5);

  return 0;
}

// 출력
// 1   2   3   4   5
// 1.1 2.2 3.3 4.4 5.5
// [오버로딩 함수]
// 1   2   3   4   5
```

위 예제처럼 char 타입을 첫번째 매개변수로 전달하면, 템플릿 함수가 호출되는 것이 아니라 오버로딩 함수가 먼저 호출됨을 알 수 있다.

# 템플릿 클래스

클래스에서 템플릿을 사용하는 방법은 템플릿 함수와 동일하다. 

템플릿 함수는 함수 내에서만의 자료형을 지정해 줬다면, 클래스 템플릿은 클래스 전체의 자료형을 결정한다.

아래는 템플릿 클래스를 활용한 스택 자료구조이다.

```cpp
#include <iostream>
using namespace std;

template <class T> class MyStack {
  int top;
  T data[100];

public:
  MyStack();
  void push(T element);
  T pop();
};

template <class T> MyStack<T>::MyStack() { top = -1; }

template <class T> void MyStack<T>::push(T element) {
  if (top == 99)
    return;
  top++;
  data[top] = element;
}

template <class T> T MyStack<T>::pop() {
  T retData;
  if (top == -1)
    return 0;

  retData = data[top--];
  return retData;
}

```

Point 클래스를 생성하여 기본 자료형부터 클래스까지 스택에 담아보려 한다.

```cpp
class Point {
  int x, y;

public:
  Point(int x = 0, int y = 0) {
    this->x = x;
    this->y = y;
  }
  void show() { cout << "(" << x << ", " << y << ")" << endl; }
};

```

```cpp

int main() {
  MyStack<int *> ipStack;
  int *p = new int[3];
  for (int i = 0; i < 3; i++)
    p[i] = i * 10;
  ipStack.push(p);
  int *q = ipStack.pop();
  for(int i = 0; i<3; i++)
    cout << q[i] << ' ';
  cout << endl;
  delete[] p;

  MyStack<Point> pointStack; // Point 객체만 저장하는 스택
  Point a(2,3), b;
  pointStack.push(a);
  b = pointStack.pop();
  b.show();

  MyStack<Point*> pStack; // Point*만 저장하는 스택
  pStack.push(new Point(10, 20));
  Point *pPoint = pStack.pop();
  pPoint->show();

  MyStack<string> stringStack;
  string s = "c++";
  stringStack.push(s);
  stringStack.push("java");
  cout << stringStack.pop() << ' ';
  cout << stringStack.pop() << endl;

  return 0;
}
```

다음은 두 개 이상의 제네릭 타입을 가진 제네릭 클래스에 대한 예제이다.

```cpp
#include <iostream>
using namespace std;

template <class T1, class T2> class GClass {
  T1 data1;
  T2 data2;

public:
  GClass();
  void set(T1 a, T2 b);
  void get(T1 &a, T2 &b);
};

template <class T1, class T2> GClass<T1, T2>::GClass() { data1 = data2 = 0; }

template <class T1, class T2> void GClass<T1, T2>::set(T1 a, T2 b) {
  data1 = a;
  data2 = b;
}

template <class T1, class T2> void GClass<T1, T2>::get(T1 &a, T2 &b) {
  a = data1;
  b = data2;
}

int main() {
  int a;
  double b;
  GClass<int, double> x;
  x.set(2, 0.5);
  x.get(a, b);
  cout << "a: " << a << ", b: " << b << endl;

  char c;
  float d;
  GClass<char, float> y;
  y.set('H', 45.5);
  y.get(c, d);
  cout << "c: " << c << ", d: " << d << endl;

  return 0;
}
```

> 출처   
명품 C++ Programming - 황기태
>
