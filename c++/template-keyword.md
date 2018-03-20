# Template keyword

템플릿에서 명시적으로 `template` 키워드를 사용해야 하는 경우에 대해 설명한다. 참고로 typename 키워드를 명시적으로 사용하는 것에 대해서도 같이 알아두는 것이 좋다. 아래 글을 참고하라.

[typename keyword]

아래와 같은 Object 클래스가 있다고 해보자.

```cpp
class Object
{
public:
    template<typename T> static void to_string() {}
    template<typename T> class Prototype {};
};
```

Object 클래스에 정의된 static 멤버함수 to_string 이나 내부 클래스인 Prototype을 접근하기 위해서는 클래스 이름이 필요하다. 그리고 이들 모두 템플릿이기 때문에 타입으로 선언하기 위해 T 타입을 결정해 주어야 한다.

```cpp
// static member function
Object::to_string<int>();

// inner class
Object::Prototype<int> a;
```

여기까지는 어려운 것이 아니다. 그러면 함수 템플릿에서 Object 클래스를 T 타입으로 받았을 경우를 생각해보자. 아래와 같은 함수 템플릿 print 를 보자.

```cpp
template<typename T> void print()
{
    T::to_string<int>();
}

int main() {
    print<Object>();
    return 0;
}
```

main 함수에서 print 함수를 사용할 때 T를 Object 클래스로 확정했고 함수 템플릿 print에서는 T를 받아 to_string 함수에 접근할 수 있다. 그런데 이는 컴파일 에러가 난다. 왜냐하면 int 앞에 `<` 기호를 operator로 해석하기 때문이다. 함수 템플릿 안에서 to_string이 멤버함수 템플릿인지 알 수가 없는 것이다. 그래서 함수 템플릿이라는 사실을 알려주기 위해 `template` 키워드가 아래와 같이 사용된다.

```cpp
template<typename T> void print()
{
    // static member function template
    T::template to_string<int>();
}
```

내부 클래스 Prototype을 print 안에서 사용할 때도 마찬가지로 < 연산자가 아니라 클래스 템플릿을 위해 사용되는 것임을 알려주기 위해 template 키워드를 사용하며, Prototype이 value가 아니라 type임을 알려주기 위한 typename 키워드도 사용해야 한다.

```cpp
template<typename T> void print()
{
    // inner class template
    typename T::template Prototype<int> b;
}
```

[typename keyword]:(https://github.com/seungin/TIL/blob/master/c%2B%2B/typename-keyword.md)
