# value_type

C++ STL Container에는 기본적으로 `value_type`이라는 것을 제공한다. 본 문서에서는 value_type이 무엇이며 STL에서 이를 제공하는 이유와 필요성에 대해서 설명한다.

```cpp
template<typename _Tp, typename _Alloc = std::allocator<_Tp> >
class list : protected _List_base<_Tp, _Alloc>
{
public:
    typedef _Tp value_type;
}
```

std::list 클래스 템플릿 정의를 일부 발췌해보면 위와 같다. 즉, `STL Container의 element 타입을 나타내는 이름이 value_type`인 것이다. 이는 다음과 같이 사용할 수 있다.

```cpp
#include <list>

int main() {
    std::list<int> list = { 1, 2, 3 };
    std::list<int>::value_type element = list.front();
    return 0;
}
```

아니 이렇게 쓸 수 있다는 것은 안다 해도 std::list\<int>::value_type이 결국은 int인데 int를 쓰면 되지 뭣하러 이렇게 길게 쓸 필요가 있다는 말인가. 그렇다, element 타입을 이미 알고 있을 때 쓰라고 만들어 둔 것이 아니다. `value_type은 템플릿으로 받은 클래스의 element 타입을 받기 위해 사용`하는 것이다. 아래 함수 템플릿을 보자.

```cpp
template<typename T>
void print_front(T container)
{
    T::value_type front = container.front();
    std::cout << front << '\n';
}

int main() {
    std::list<int> list = { 1, 2, 3 };
    print_front(list);
    return 0;
}
```

이렇게 쓰라고 있는 것이 value_type 이다. 그런데! 컴파일 에러가 날 것이다. T::value_type 이라고만 쓰면 [컴파일러는 기본적으로 값으로 해석](https://github.com/seungin/TIL/blob/master/c%2B%2B/typename-keyword.md)하기 때문이다. 아래와 같이 typename 키워드를 넣어주어야 한다.

```cpp
template<typename T>
void print_front(T container)
{
    typename T::value_type front = container.front();
    std::cout << front << '\n';
}
```

아니면 더 편한 방법으로는, C++11 부터 추가된 `auto` 키워드를 이용하면 typename 키워드를 적어주지 않아도 되므로 매우 편리하다. (하지만 그렇다 하더라도 typename 키워드를 사용해야 하는 때를 아는 것은 매우 중요하다)

```cpp
template<typename T>
void print_front(T container)
{
    auto front = container.front();
    std::cout << front << '\n';
}
```
