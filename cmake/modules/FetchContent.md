# cmake-modules(7) : FetchContent

빌드하고자 하는 소스를 다운로드 하기 위해 사용하는 모듈이다. ExternalProject_Add 모듈은 build time에 수행되지만 FetchContent 모듈은 configure time에 처리하는 것이 특징이다.

`FetchContent_Declare( PackageName )` 를 이용하여 package를 얻어올 URL, Tag 이름 등을 먼저 설정한다. 그런 다음 CMake3.14+ 이라면 `FetchContent_MakeAvailable( PackageName )` 를 이용하여 package를 다운로드하고 빌드 준비를 마칠 수 있다. 만약 이전 CMake 버전을 이용하고 있거나 빌드를 위해 추가적인 Custom 작업이 필요하다면 `FetchContent_GetProperties( PackageName )` 와 `FetchContent_Populate( PackageName )` 를 이용하면 된다.

## Examples

GoogleTest를 사용하는 간단한 FetchContent 예제를 보면 아래와 같다.

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

Protobuf 의 경우에는 package를 얻어오면 Top-Level 폴더에 CMakeLists.txt 파일이 존재하지 않는다. 이런 경우에도 FetchContent_MakeAvailable 을 이용할 수 없다. 그러므로 다음과 같이 사용하면 된다.

```cmake
# CMakeLists.txt

cmake_minimum_required( VERSION 3.15 )

project( UsingProtobuf )

include( FetchContent )

FetchContent_Declare(
  protobuf
  GIT_REPOSITORY  https://github.com/protocolbuffers/protobuf.git
  GIT_TAG         v3.6.1
)

FetchContent_GetProperties( protobuf )
if( NOT protobuf_POPULATED )
  FetchContent_Populate(protobuf)
  add_subdirectory(${protobuf_SOURCE_DIR}/cmake ${protobuf_BINARY_DIR})
endif()
```
