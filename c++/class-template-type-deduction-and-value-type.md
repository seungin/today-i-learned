# Class template type deduction and value_type

클래스 템플릿 타입 추론에서 value_type, typename이 어떻게 사용되는지를 보며 value_type, typename을 좀 더 깊이 이해해 보고자 한다. 먼저 아래와 같이 Vector라는 간단한 컨테이너를 클래스 템플릿으로 구현해보자.

```cpp
template<typename T> class Vector
{
    T buffer;
    size_t size;

public:
    Vector(int size, T value) {}
};
```

이렇게 구현한 Vector를 STL의 list로 초기화하는 생성자를 구현해 보자.

```cpp
template<typename T> class Vector
{
    T buffer;
    size_t size;

public:
    //TODO: STL list로부터 Vector를 만드는 생성자를 구현해 봅시다
    Vector(???) {}
};

int main() {
    std::list<int> list = { 1, 2, 3 };
    Vector vector(list);
    return 0;
}
```
