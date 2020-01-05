# cmake-variables(7) : CMAKE_PROJECT_INCLUDE_BEFORE

project 명령을 수행하기 전에 항상 include 할 파일을 지정한다.

## Examples

```cmake
# common-project-include.cmake

set(ENABLE_TESTING TRUE)
```

```cmake
# CMakeLists.txt

cmake_minimum_required(VERSION 3.15)

set(CMAKE_PROJECT_INCLUDE
  "${CMAKE_CURRENT_LIST_DIR}/common-project-include.cmake")

project(MyProject)
message(STATUS "ENABLE_TESTING: ${ENABLE_TESTING}")
```

cmake 실행 결과는 아래와 같다.

> ENABLE_TESTING: TRUE
