# dynamic_cast

# Upcast와 Downcast

dynamic cast에 대해 설명하기 앞서 Upcast와 Downcast에 대해 간략하게 설명하고자 한다.

- 업 캐스트: Derived 클래스에서 Base 클래스로 변환하는 것
- 다운 캐스트: Base 클래스에서 Derived 클래스로 변환하는 것

자세한 내용은 해당 [포스트](https://velog.io/@jwlee010523/inheritance)에 자세하게 설명되어 있다.
# dynamic_cast란

```cpp
dynamic_ast<type_id>(expression)
```

dynamic_cast는 부모 클래스의 포인터에서 자식 클래스의 포인터로 다운 캐스팅 해 주는 연산자이다. 업캐스팅에도 쓰이지만 주로 safe downcasting(안전한 다운캐스팅)을 위해사용된다. 참조나 포인터에 대해서만 사용이 가능하고, RTTI 런타임 형식 정보를 사용하기 때문에 비용이 조금 높은 연산자이다.

type_id는 포인터나 래퍼런스이어야 한다. 표현식 exp는 type_id가 포인터인 경우에는 객체에 대한 주소값을 가져야 하고, 래퍼런스인 경우에는 I-value이어야 한다.

성공할 경우에는 type_id의 value를 반환한다. 실패할 경우에는 nullptr를 반환합니다.

## dynamic_cast 예제 1

```cpp
#include <iostream>
using namespace std;

class Blog {
public:
  Blog() { cout << "Blog()\n"; }
  virtual ~Blog() { cout << "~Blog()\n"; }
  void Show() { cout << "This is Blog calss\n"; }
};

class Velog : public Blog {
public:
  Velog() { cout << "Velog\n"; }
  virtual ~Velog() { cout << "~Velog\n"; }
  void Show() { cout << "This is Velog class\n"; }
};

int main() {
  Blog *pBlog = new Velog();
  pBlog->Show();

  Velog* pVelog = dynamic_cast<Velog*>(pBlog);
  if(pVelog == nullptr) {
    cout << "Runtime error\n";
  } else {
    pVelog->Show();
  }
  delete pBlog;
  
  return 0;
}

// 출력
// Blog()
// Velog
// This is Blog calss
// This is Velog class
// ~Velog
// ~Blog()
```

여기서 중요한 점은 부모 클래스의 포인터로 자식 클래스를 가리킬 수 있다는 것이다. 또한 pBlog→Show()는 실제 pBlog가 가리키는 클래스는 본인의 자식 클래스인 Velog 클래스이지만, virtual로 선언되어 있지 않기 때문에 pBlog가 가지고 올 수 있는 것은 본인의 Show() 함수이다.  만약 virtual로 선언되어 있었다면, 자식 클래스의 Show() 함수를 호출했을 것이다.

virtual에 관한 설명은 해당 [포스트](https://velog.io/@jwlee010523/다형성Polymorphism)에 설명되어 있다.

마지막으로 제일 중요한 점은 pBlog가 실제 가리키는 클래스(동적할당 된 클래스)는 자기 자식인 Velog 클래스이므로 dyanamic_cast를 통해 다운캐스팅이 가능하다.

## dynamic_cast 예제 2

```cpp
#include <iostream>
using namespace std;

class Blog {
public:
  Blog() { cout << "Blog()\n"; }
  virtual ~Blog() { cout << "~Blog()\n"; }
  void Show() { cout << "This is Blog calss\n"; }
};

class Velog : public Blog {
public:
  Velog() { cout << "Velog\n"; }
  virtual ~Velog() { cout << "~Velog\n"; }
  void Show() { cout << "This is Velog class\n"; }
};

int main() {
  Blog *pBlog = new Blog();
  pBlog->Show();

  Velog* pVelog = dynamic_cast<Velog*>(pBlog);
  if(pVelog == nullptr) {
    cout << "Runtime error\n";
  } else {
    pVelog->Show();
  }
  delete pBlog;
  
  return 0;
}

// 출력
// Blog()
// This is Blog calss
// Runtime error
// ~Blog()
```

이번에는 예제 1과 다르게 런타임 에러가 발생했다. 그 이유는 Velog 클래스의 생성자가 한번도 호출된 적이 없는데 다운 캐스팅을 진행하려고 했기 때문이다.

> 출처   
https://blockdmask.tistory.com/241   
https://www.youtube.com/사람만이
>
