# Nontype template parameter

템플릿 파라미터로 사용될 수 있는 값들에는 어떤 것들이 있는지 알아본다. 크게 4가지가 있는데 `정수형 상수`, `enum 상수`, `포인터`, 그리고 `함수 포인터`가 있다. 하나씩 차례대로 그 사용법을 보도록 하자. (다른 템플릿 파라미터를 보고 싶으면 아래 링크를 참고하라)

[template parameter]

```cpp
// 1. 정수형 상수
template<int N> class Array {};

int main() {
    Array<10> array;
}
```

```cpp
// 2. enum 상수
enum Color { red, green, blue };
template<Color> class Font {};

int main() {
    Font<red> font;
}
```

```cpp
// 3. 포인터
template<int*> class Address {};
int g_var;

int main() {
    Address<&g_var> address;
}
```

```cpp
// 4. 함수 포인터
template<void(*)(const char*)> class Delegate {};
void hello(const char* name) { std::cout << name << '\n'; }

int main() {
    Delegate<hello> delegate;
}
```

```cpp
// 5. auto
template<auto T> struct Type
{
    Type() { std::cout << typeid(T).name() << '\n'; }
};

int main() {
    // since C++17
    Type<10> intType;
    Type<&g_var> ptrType;
    Type<&main> funcType;
    return 0;
}
```

[template parameter]:https://github.com/seungin/TIL/blob/master/c%2B%2B/template-parameter.md
