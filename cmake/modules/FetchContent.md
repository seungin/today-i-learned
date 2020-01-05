# cmake-modules(7) : FetchContent

빌드하고자 하는 소스를 다운로드 하기 위해 사용하는 모듈이다. ExternalProject_Add 모듈은 build time에 수행되지만 FetchContent 모듈은 configure time에 처리하는 것이 특징이다.

## Examples

GoogleTest를 사용하는 예제를 보면 아래와 같다.

```cmake
# external/googletest/CMakeLists.txt

# Load FetchContent module
include( FetchContent )

# Declare GoogleTest as the content to fetch
FetchContent_Declare(
  googletest
  GIT_REPOSITORY  https://github.com/google/googletest.git
  GIT_TAG         release-1.8.1
)

# Fetch GoogleTest and make build scripts available
FetchContent_MakeAvailable( googletest )
```

```cmake
# CMakeLists.txt
cmake_minimum_required( VERSION 3.15 )

project( Unittest )

add_subdirectory( external/googletest )

add_executable( ${PROJECT_NAME} main.cpp )
target_link_libraries( ${PROJECT_NAME} PRIVATE gtest gtest_main )
```

```cpp
#include "gtest/gtest.h"

class SampleTest : public ::testing::Test
{
};

TEST_F(SampleTest, ConsoleOut)
{
	EXPECT_EQ(1 + 1, 2);
}
```

Unittest.exe 실행 결과는 아래와 같다.

> [==========] Running 1 test from 1 test case.  
> [----------] Global test environment set-up.  
> [----------] 1 test from SampleTest  
> [ RUN      ] SampleTest.ConsoleOut  
> [       OK ] SampleTest.ConsoleOut (0 ms)  
> [----------] 1 test from SampleTest (1 ms total)  
>   
> [----------] Global test environment tear-down  
> [==========] 1 test from 1 test case ran. (2 ms total)  
> [  PASSED  ] 1 test.  
