# cmake-commands(7) : configure_file

입력 템플릿 파일의 내용을 채워 결과 파일을 지정된 위치로 복사한다.

## Examples

- 입력 파일의 기본 경로는 ${CMAKE_CURRENT_SOURCE_DIR} 이다.
- 출력 파일의 기본 경로는 ${CMAKE_CURRENT_BINARY_DIR} 이다.

```cmake
# Top-Level CMakeLists.txt
project(Foo VERSION 1.0.0)

option(TEST_ENABLE "Enable Test Mode" ON)

configure_file(
  version.hpp.in
  version.hpp
)
```

입력 파일 version.hpp.in 에 대하여

```hpp.in
#cmakedefine TEST_ENABLE
#cmakedefine01 TEST_ENABLE
#define Foo_VERSION "@Foo_VERSION_MAJOR@.@Foo_VERSION_MINOR@.@Foo_VERSION_PATCH@"
```

출력 파일 version.hpp 는 옵션 적용에 따라 아래와 같은 두 내용으로 채워진다.

```hpp
#define TEST_ENABLE
#define TEST_ENABLE 1
#define Foo_VERSION "1.0.0"
```

```hpp
/* #undef TEST_ENABLE */
#define TEST_ENABLE 0
#define Foo_VERSION "1.0.0"
```
