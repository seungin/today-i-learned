# Template and friend function

C++ 템플릿에서 friend 함수를 사용하는 방법을 operator<< 를 구현하며 설명하겠다. 먼저 클래스 템플릿 Point를 << 연산자를 이용해서 std::cout으로 출력하는 구현을 보자.

```cpp
#include <iostream>

template<typename T> class Point
{
    T x;
    T y;

public:
    Point(T x, T y) : x(x), y(y) {}
};

template<typename T>
std::ostream& operator<<(std::ostream& os, Point<T>& p)
{
    // ERRROR! p.x에 접근할 수 없음!
    std::cout << p.x << ", " << p.y;
}

int main() {
    Point<int> point(1, 2);
    std::cout << point << '\n';
    return 0;
}
```

이 코드는 컴파일 에러가 발생한다. 그 이유는 operator<< 에서 Point 클래스의 private 멤버에 접근할 수 없기 때문이다. 이것을 해결하려면 Point 클래스에서 operator<< 에 대한 friend를 선언해야 한다.

```cpp
template<typename T> class Point
{
    ...

public:
    friend std::ostream& operator<<(std::ostream& os, const Point<T>& p);
};
```

어라? 그런데 에러가 발생한다. 왜 그럴까? 에러 메시지를 한번 보자.

```txt
main.cpp:23: undefined reference to `operator<<(std::ostream&, Point<int> const&)'
```

흠.. 우리는 함수 템플릿을 정의했으므로 Point\<int> 타입에 해당하는 함수가 생성되어야 마땅해 보이는데 왜 함수 정의를 찾지 못할까? 그 이유는 바로 friend를 잘못 선언했기 때문이다. 우리는 함수 템플릿에 대해 friend를 선언했다고 생각했지만, 사실은 Point\<int> 클래스가 생성될 때 operator<< 함수의 parameter도 Point\<int> 타입으로 고정되기 때문에 특정 타입의 함수의 선언한 것이다. 특정 타입이 선언되었으니 함수 템플릿은 함수를 생성하지 않을 것이고 그래서 정의가 없다는 컴파일 에러가 발생하는 것이다. 이를 해결한 코드를 한번 보자.
