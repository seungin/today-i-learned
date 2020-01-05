# cmake-commands(7) : set_target_properties

target이 빌드될 때 영향을 주는 속성(Property)을 부여한다.

## Examples

부여할 속성과 그에 대응하는 값을 한 쌍으로 하여 여러 개를 설정할 수 있다.

```cmake
add_executable(Foo main.cpp)
set_target_properties(Foo PROPERTIES
  <Prop1> <Value1>
  <Prop2> <Value2>
)
```

## Properties on Targets

```cmake
POSITION_INDEPENDENT_CODE ON
```

-fPIC 링커 옵션을 부여한다.

```cmake
VERSION "1.0.0"
SOVERSION "1"
```

공유 라이브러리 이름을 결정하는데 사용된다. 예를 들어, 위의 속성으로 foo 라이브러리를 빌드하면 아래와 같이 linker name은 libfoo.so, so name은 libfoo.so.1, real name은 libfoo.so.1.0.0 으로 결과물이 생성된다.

- libfoo.so -> libfoo.so.1
- libfoo.so.1 -> libfoo.so.1.0.0
- libfoo.so.1.0.0

수많은 속성들이 있지만 추후 사용법을 알게 될 때 업데이트할 예정
