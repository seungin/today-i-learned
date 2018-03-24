# Template template parameter

템플릿 파라미터로 템플릿이 사용될 수도 있다. 사실 이 방법은 매우 많이 사용되는 방식인데, 이에 대한 사용법을 보도록 하자. (다른 템플릿 파라미터를 보고 싶으면 아래 링크를 참고하라)

[template parameter]

아래는 간단한 List 클래스를 템플릿으로 구현한 것이다.

```cpp
template<typename T> class List {};

int main() {
  List<int> list;
}
```

위 List 클래스를 템플릿 인자로 넘겨줄 수 있도록 컨테이너를 템플릿으로 하는 Stack 클래스를 만들어보자.

```cpp
template<typename T, template<typename> class Container> Stack
{
  Container<T> buffer;
};

int main() {
  Stack<int, List> stack;
}
```

혹시 이렇게 사용하는 것이 어렵게 느껴질 수 있는데 사실, Stack 의 두 번째 템플릿 파라미터 Container는 클래스 템플릿 List와 형태가 거의 같다.

```cpp
template<typename T> class List
template<typename> class Container
```

다만 템플릿 인자로 하나만 받는다는 의미에서 `template<typename>` 이라고 써 주고 클래스 템플릿이므로 `class Container` 라고 써 주는 것이다.

[template parameter]:https://github.com/seungin/TIL/blob/master/c%2B%2B/template-parameter.md
