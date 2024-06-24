# 다중 상속

```cpp
class MP3 {
public:
  void play();
  void stop();
};

class MobilePhone {
public:
  bool sendCall();
  bool receiveCall();
  bool sendSMS();
  bool receiveSMS();
};

class MusicPhone : public MP3, public MobilePhone {
public:
  void dial();
};
```

다중 상속은 접근 지정과 함께 기본 클래스를 콤마(,)로 나열하면 된다.

## 다중 상속의 활용

파생 클래스는 다중 상속받은 기본 클래스들의 멤버들을 모두 호출할 수 있기 때문에, MusicPhone 클래스의 dial 함수는 아래와 같이 구현할 수 있다.

```cpp
void MusicPhone::dial() {
	play();
	sendCall();
}
```

## 다중 상속의 문제점

다중 상속은 여러 개의 클래스를 상속받음으로써 클래스의 재사용과 코딩의 효율을 높이는 장점이 있지만, 치명적인 문제점을 내포하고 있다.

![image](https://github.com/jwon0523/Today-I-Learned/assets/50106190/48fc7ff6-58f3-4141-a493-07532f2c2238)

위 그림에 나와있다시피 ioObj 객체에 mode가 2개이기 때문에 컴파일러는 어떤 mode 변수를 호출하는 것인지 모호하기 때문에 오류를 발생시킨다.

이처럼 다이아몬드 형의 다중 상속 구조는 기본 클래스의 멤버가 중복 상속되어 객체 속에 존재하는 상황을 초래하여 컴파일 오류를 발생시킨다.

InOut 클래스 내에서 mode를 접근할 때도 마찬가지로, 컴파일 오류가 발생한다.

```cpp
class InOUt : public In, public Out {
public:
	bool safe;
	void setMode() { mode = 3; } // mode에 대한 모호성으로 인한 컴파일 오류 발생
}
```

# 가상 상속

다중 상속에서 생기는 멤버 중복 생성 문제를 해결하려면, 파생 클래스를 선언할 때 기본 클래스 앞에 virtual 키워드를 이용하여 가상 상속을 선언하면 된다.

```cpp
class In : virtual public BaseIO {
	...
};

class Out : virtual public BaseIO {
	...
};
```

이렇게 하면 In과 Out은 가상 클래스를 상속 받을 수 있다.

virtual 키워드는 컴파일러에게 파생 클래스의 객체가 생성될 때 기본 클래스의 멤버 공간을 오직 한 번만 할당하고, 이미 할당되어 있다면 그 공간을 공유하도록 지시한다.

![image](https://github.com/jwon0523/Today-I-Learned/assets/50106190/1ce149cf-a204-40d3-9d07-6e84e3798fb2)

> 출처   
명품 C++ Programming - 황기태
>
