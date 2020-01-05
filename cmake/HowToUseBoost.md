# How to use Boost library

cmake를 이용할 때 Boost 라이브러리를 사용하는 방법을 설명한다. 단 여기에서 설명하는 find_package의 `CONFIG` 모드는 BoostConfig.cmake를 제공하기 시작한 `Boost 1.70 이상`에서만 가능하다. 이전 버전을 이용하려면 `MODULE` 모드를 이용해야 하며 아래 동영상을 참고하기 바란다.

[Oh No! More Modern CMake, Deniz Bahadir - Meeting C++ 2019](https://youtu.be/y9kSr5enrSk)


## Examples

GoogleTest를 사용하는 예제를 보면 아래와 같다.

```cmake
# external/boost/CMakeLists.txt

set( BOOST_VERSION 1.70.0 )

# Settings for finding correct Boost libraries
set( Boost_USE_STATIC_LIBS      FALSE )
set( Boost_USE_MULTITHREADED    TRUE )
set( Boost_USE_STATIC_RUNTIME   FALSE )
set( Boost_COMPILER             "-gcc8" )

# Search for Boost libraries
find_package( Boost ${BOOST_VERSION} EXACT CONFIG REQUIRED COMPONENTS
  atomic
  chrono
  thread )

# Make found targets globally available
if( Boost_FOUND )
  set_target_properties(
    Boost::atomic
    Boost::chrono
	Boost::thread
    PROPERTIES IMPORTED_GLOBAL TRUE )
endif()
```

```cmake
# CMakeLists.txt

cmake_minimum_required( VERSION 3.15 )

project( BoostTest )

add_subdirectory( external/boost )

add_executable( ${PROJECT_NAME} main.cpp )
target_link_libraries( ${PROJECT_NAME} PRIVATE Boost::atomic Boost::chrono Boost::thread )
```

```cpp
#include <iostream>
#include <boost/thread.hpp>

int main()
{
    std::cout << "wait for 1 second" << std::endl;
    boost::this_thread::sleep_for(boost::chrono::seconds(1));
    std::cout << "timeout!" << std::endl;
}
```

BoostTest.exe 실행 결과는 아래와 같다.

> wait for 1 second  
> timeout!  
