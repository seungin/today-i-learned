# How to use Protobuf library

cmake를 이용할 때 Protobuf 라이브러리를 사용하는 방법을 설명한다. Protobuf는 다른 라이브러리들과는 다르게 IDL 파일이 존재한다. 그리고 protoc 이라는 컴파일러가 IDL을 이용하여 대상 언어(여기에서는 cpp) message 파일로 생성한다. 이러한 기능을 어떻게 cmake에서 사용할 수 있는지도 설명한다.

## Install

Protobuf를 설치하는 방법을 설명한다. cmake 모듈 중 FetchContent 를 이용하면 쉽게 설치가 가능하다. git repository Top-Level 폴더에 CMakeLists.txt 파일이 존재하지 않기 때문에 FetchContent_MakeAvailable 명령은 동작하지 않는다. FetchContent_GetProperties, FetchContent_Populate 명령을 이용하도록 한다.

```cmake
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

## Examples

Protobuf를 사용하는 예제를 보면 아래와 같다.

만약 Window 환경이라면 `PROTOBUF_ROOT` 라는 환경변수가 설치된 경로로 설정되어 있어야 한다.

```cmd
> set PROTOBUF_ROOT=C:\Local\protobuf
```

```cmake
# external/protobuf/CMakeLists.txt

if( POLICY CMP0074 )
  cmake_policy( SET CMP0074 NEW )
endif()

find_package( Protobuf REQUIRED )

message( STATUS "Protobuf_FOUND: ${Protobuf_FOUND}" )
message( STATUS "Protobuf_VERSION: ${Protobuf_VERSION}" )
message( STATUS "Protobuf_INCLUDE_DIRS: ${Protobuf_INCLUDE_DIRS}" )
message( STATUS "Protobuf_LIBRARIES: ${Protobuf_LIBRARIES}" )
message( STATUS "Protobuf_PROTOC_LIBRARIES: ${Protobuf_PROTOC_LIBRARIES}" )
message( STATUS "Protobuf_LITE_LIBRARIES: ${Protobuf_LITE_LIBRARIES}" )
```

```cmake
cmake_minimum_required( VERSION 3.15 )

project( UsingProtobuf )

add_subdirectory( external/protobuf )

# 추가 수정 예정
# protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS foo.proto)
# protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS EXPORT_MACRO DLL_EXPORT foo.proto)
# protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS DESCRIPTORS PROTO_DESCS foo.proto)
# protobuf_generate_python(PROTO_PY foo.proto)
# add_executable(bar bar.cc ${PROTO_SRCS} ${PROTO_HDRS})
# target_include_directories( ${PROJECT_NAME} PRIVATE ${Protobuf_INCLUDE_DIRS} )
# target_link_libraries( ${PROJECT_NAME} PRIVATE ${Protobuf_LIBRARIES} )
```

```cpp
# 추가 구현 예정
# ...
```

UsingProtobuf.exe 실행 결과는 아래와 같다.

> # 추가 구현 예정  
> # ...  
