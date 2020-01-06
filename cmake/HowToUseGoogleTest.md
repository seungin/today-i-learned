# How to use GoogleTest library

cmake를 이용할 때 GoogleTest 라이브러리를 사용하는 방법을 설명한다. GoogleTest는 소스 그대로 다운로드받아 빌드해도 오래 걸리지 않으므로 이 방식을 이용한다.

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

project( UsingGoogleTest )

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

UsingGoogleTest.exe 실행 결과는 아래와 같다.

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
