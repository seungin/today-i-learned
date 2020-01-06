# cmake-commands(7) : find_package

설치된 외부 package를 찾아 설정들을 로드한다. 보통 Find\<PackageName\>.cmake 파일을 제공해주는 외부 라이브러리들을 찾을 때 사용할 수 있다.

Linux에서는 /usr/local/lib에 라이브러리가 설치되어 있으면 package를 찾을 수 있다. 불행하게도 Windows는 기본 검색 경로는 존재하지 않는다. 다만 CMake 3.12부터 도입된 CMP0074 정책을 적용한 후 환경변수 %\<PackageName\>_ROOT% 를 설치된 경로로 설정해두면 package를 찾을 수 있다.

## Examples

Windows 환경 C:\Local\protobuf 경로에 설치된 Protobuf 라이브러리를 다음과 같이 찾을 수 있다.

먼저 아래와 같이 환경변수를 설정한다.

```cmd
> set PROTOBUF_ROOT=C:\Local\protobuf
```

환경 변수를 이용할 것이므로 CMP0074 정책을 적용한다.

```cmake
# Top-Level CMakeLists.txt

project( UsingProtobuf )

if( POLICY CMP0074 )
  cmake_policy( SET CMP0074 NEW )
endif()

find_package( protobuf REQUIRED )

message( STATUS "Protobuf_FOUND: ${Protobuf_FOUND}" )
message( STATUS "Protobuf_VERSION: ${Protobuf_VERSION}" )
message( STATUS "Protobuf_INCLUDE_DIRS: ${Protobuf_INCLUDE_DIRS}" )
message( STATUS "Protobuf_LIBRARIES: ${Protobuf_LIBRARIES}" )
message( STATUS "Protobuf_PROTOC_LIBRARIES: ${Protobuf_PROTOC_LIBRARIES}" )
message( STATUS "Protobuf_LITE_LIBRARIES: ${Protobuf_LITE_LIBRARIES}" )
```
