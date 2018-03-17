# Generic copy constructor

C++ 템플릿 클래스에서 복사 생성자를 구현하는 몇 가지 케이스를 살펴본다. 기본적인 복사 생성자는 자신과 동일한 타입인 Complex<T> 를 인자로 받는다. 다른 타입을 인자로 받고 싶으면 T 자리 대신에 고정타입(예를 들면 int라든지)을 사용할 수도 있다. 하지만 모든 타입의 Complex를 인자로 받기 위해서는 멤버함수 템플릿을 이용해야 하는데, 이것이 `Generic copy constructor` 이다.

```cpp
template<typename T> class Complex
{
    T real;
    T imaginary;

public:
    Complex(T real, T imaginary) {}
    Complex(const Complex<T>& c) {}                      // 복사 생성자
    Complex(const Complex<int>& c) {}                    // 고정타입 복사 생성자
    template<typename U> Complex(const Complex<U>& c) {} // 모든타입 복사 생성자
};

int main() {
    {
        Complex<char> c1(1, 0);
        Complex<char> c2 = c1; // OK. 동일한 타입(T는 char)
    }

    {
        Complex<int> c1(1, 0);
        Complex<float> c2 = c1;  // OK. 고정 타입(int)
        Complex<double> c3 = c1; // OK. 고정 타입(int)
    }

    {
        Complex<char> c1(1, 0);
        Complex<int> c2 = c1;   // OK. 모든 타입 가능
        Complex<double> c3 = c2;  // OK. 모든 타입 가능
    }

    return 0;
}
```

Generic copy constructor를 구현할 때 parameter의 private 멤버에 접근하고 싶을 수 있다. 예를 들면 아래 코드와 같이 c.real, c.imaginary 값에 접근하고 싶을 것이다.

```cpp
template<typename T> class Complex
{
    T real;
    T imaginary;

public:
    Complex(T real, T imaginary) {}
    template<typename U> Complex(const Complex<U>& c) : real(c.real), imaginary(c.imaginary) {}
};

int main() {
    Complex<int> c1(1, 0);
    Complex<double> c2 = c1; // ERROR! c1의 private 멤버에 접근할 수가 없음!

    return 0;
}

```

그런데 Complex<double> 타입과 Complex<int> 타입은 서로 다른 타입이므로 상대방의 private 멤버에는 접근할 수 없기 때문에 컴파일 에러가 발생한다. 이를 해결하기 위해서는 Complex 클래스를 정의할 때 모든 타입의 Complex 클래스에 대한 friend 선언이 필요하다.

```cpp
template<typename T> class Complex
{
    ...

public:
    template<typename> friend class Complex;
};
```
