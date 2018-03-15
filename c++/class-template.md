# Class template

C++ 템플릿은 두 종류이다. 함수 템플릿과 클래스 템플릿이다. 그 중 본 문서에서는 클래스 템플릿에 대해 설명한다. 간단한 내용이지만 기본을 잘 숙지하자.

## Template? Type?

한 가지 중요한 개념은 템플릿과 타입을 구분하는 것이다. 아래와 같은 Complex 클래스가 있다고 할 때, Complex는 템플릿이며, Complex<int>, Complex<double> 은 타입이다. 그 차이는 객체를 생성할 수 있는 구체적인 구현이 만들어져 있는가 아닌가이다.

```cpp
template<typename T>
class Complex
{
    T real;
    T imaginary;

public:
    // T를 써 주는 것이 정확한 표현이지만
    void foo(Complex<T> param) {
        Complex<T> var;
    }

    // 클래스 정의 안에서는 T를 생략해도 무방
    void goo(Complex param) {
        Complex var;
    }

};

int main() {
    Complex c1;       // ERROR! Complex는 타입이 아니라 템플릿이므로 객체를 만들 수 없다.
    Complex<int> c2;  // OK. Complex<int>는 구체적인 타입이므로 객체 생성이 가능하다.
    return 0;
}
```

## Template Argument Default Values

템플릿이 아닌 일반 Complex 클래스의 생성자에서 double real 멤버 변수를 초기화할 때 보통 0을 많이 쓴다. 하지만 템플릿에서는 0으로 초기화할 수 없는 타입이 들어올 수도 있기 때문에 이러한 초기화는 불가능하다. 그럼 어떻게 일반적인 초기화를 할 수 있을까? C++98/03에서는 T()를 사용했고, C++11부터는 `Brace Initialization` 또는 `Uniform Initialization` 라고 부르는 {} 중괄호 표현이 가능하다.

```cpp
template<typename T>
class Complex
{
    T real;
    T imaginary;

public:
    Complex(T param1 = 0) {}                       // 템플릿에서는 0 초기화가 안 되는 경우가 많다 
    Complex(T param1, T param2 = T()) {}           // C++98/03 스타일 : T()
    Complex(T param1, T param2, T param3 = {}) {}  // C++11 스타일 : {}
};
```
