# Template parameter

템플릿 파라미터로 사용될 수 있는 것들에는 어떤 것들이 있는지 알아보도록 하자. 크게 3가지로 분류될 수 있는데 `타입`이나 `값(nontype)`, 그리고 `템플릿`을 받을 수 있다. 각각에 대해서 상세히 알아보기로 하자.

## Type

템플릿을 사용할 때 parameter의 대부분은 타입이다. 예를 들면 아래 코드와 같이 int를 넘기는 것이 타입을 넘기는 것이다. 너무 간단한 내용이므로 이 정도로만 하겠다.

```cpp
template<typename T> class List
{
    std::vector<T> buffer;
};

int main()
{
    List<int> list;
}
```

## Value (Nontype)

템플릿 파라미터로 값을 넘길 수도 있는데 이에 대해서는 아래 문서를 참고하자.

[nontype template parameter]

## Template

템플릿 파라미터로 템플릿을 넘길 수도 있다. 아래 문서를 참고하자.

[template template parameter]

[nontype template parameter]:https://github.com/seungin/TIL/blob/master/c%2B%2B/nontype-template-parameter.md
[template template parameter]:https://github.com/seungin/TIL/blob/master/c%2B%2B/template-template-parameter.md
