# Nontype template parameter

템플릿 파라미터로 사용될 수 있는 값들에는 어떤 것들이 있는지 알아본다. 크게 4가지가 있는데 `정수형 상수`, `enum 상수`, `포인터`, 그리고 `함수 포인터`가 있다. 하나씩 차례대로 그 사용법을 보도록 하자. (다른 템플릿 파라미터를 보고 싶으면 아래 링크를 참고하라)

[template parameter]

정수형 상수를 이용하는 경우이다.

```cpp
// 1. 정수형 상수
template<int N> class Array {};

int main() {
    Array<10> array;
}
```

enum 상수를 이용하는 경우이다.

```cpp
// 2. enum 상수
enum Color { red, green, blue };
template<Color> class Font {};

int main() {
    Font<red> font;
}
```

포인터도 사용할 수 있다.

```cpp
// 3. 포인터
template<int*> class Address {};
int g_var;

int main() {
    Address<&g_var> address;
}
```

함수 포인터도 사용할 수 있다.

```cpp
// 4. 함수 포인터
template<void(*)(const char*)> class Delegate {};
void hello(const char* name) { std::cout << name << '\n'; }

int main() {
    Delegate<hello> delegate;
}
```

C++17 이후부터는 auto도 사용 가능하다.

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

참고로 gcc를 이용하는 경우에 실행 결과는 아래와 같이 나온다.

> i  
> Pi  
> PFivE  

이를 보기 좋은 모양으로 출력하려면 c++filt 명령을 이용하면 된다. 바이너리 이름이 a.out 이라면 `a.out | c++filt -t` 와 같이 사용할 수 있다.

> int  
> int*  
> int (*)()  

[template parameter]:https://github.com/seungin/TIL/blob/master/c%2B%2B/template-parameter.md
