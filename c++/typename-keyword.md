# Typename keyword

템플릿에서 `typename` 키워드에 대해 설명한다. 이는 단순히 템플릿을 정의할 때 사용하는 template\<typename T>를 말하는 것이 아니다. 명시적으로 typename 키워드를 사용해야 하는 케이스에 대해 말할 것이다. 그 전에 먼저 클래스 이름으로 접근하는 요소들에는 어떤 것들이 있는지 살펴보자.

```cpp
class Data
{
public:
    /* values */

    enum { DWORD };    // 1. enum 상수
    static int DWORD;  // 2. static 멤버변수

    /* types */

    typedef int DWORD; // 3. typedef
    using DWORD = int; // 4. using (type alias)
};
```

enum 상수, static 멤버변수와 같은 `값`도 클래스 이름으로 접근하고, typedef나 using과 같은 `타입`도 클래스 이름으로 접근한다. 그렇다면 아래 함수 템플릿 print를 보자.

```cpp
template<typename T>
void print(T param) {
    // 해석해보라. T::DWORD는 타입일까, 값일까?
    T::DWORD * a;
}
```

위 코드는 함수 템플릿의 인자로 들어온 T의 DWORD를 접근하고 있다. 그런데 우리는 클래스 이름으로 접근 가능한 것이 값일 수도, 타입일 수도 있다는 것을 안다. 값이라면 두 값의 곱셈 연산이 될 테고, 타입이라면 포인터 변수의 선언이 될 것이다. 즉 컴파일러도 위 코드를 판별할 수 있는 방법이 없기 때문에 `기본적으로 T::DWORD를 값으로 해석`한다. 그리고 만약 이를 타입으로 해석해야 한다면 `typename 키워드를 그 앞에 반드시 붙여주어야`한다.

```cpp
template<typename T>
void print(T param) {
    T::DWORD * a;          // 값으로 해석
    typename T::DWORD * a; // 타입으로 해석
}
```

한 가지 주의할 점이 있는데, 템플릿이 아닐 경우에는 typename 키워드를 붙이면 컴파일 에러가 난다는 사실을 기억하길 바란다.

```cpp
typename T::DWORD* a;     // OK
typename Data::DWORD * a; // ERROR!
```
