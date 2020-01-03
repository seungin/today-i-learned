# cmake-variables(7) : CMAKE_PROJECT_INCLUDE_BEFORE

project 명령을 수행하기 전에 항상 include 할 파일을 지정한다.

> 그런데 버그인지 내 실수인지 문제가 있다.  
> 아래와 같이 구성했을 때 제대로 include 한 파일의 variable을 읽어오지 못한다.  
> 테스트 해보면 project 명령을 2번 했을 때 제대로 variable을 읽어온다.  
> 추후 확인될 시 수정 요망

## Examples

```cmake
# CMakeLists.txt

cmake_minimum_required( VERSION 3.15...3.17 )

set( CMAKE_PROJECT_INCLUDE_BEFORE
  "${CMAKE_CURRENT_LIST_DIR}/common-project-include.cmake" )

project(MyProject
  VERSION ${project_version}
  DESCRIPTION ${project_description}
  HOMEPAGE_URL ${project_homepage}
  LANGUAGES C CXX )

message(STATUS "MyProject_VERSION_MAJOR: ${MyProject_VERSION_MAJOR}")
message(STATUS "MyProject_VERSION_MINOR: ${MyProject_VERSION_MINOR}")
message(STATUS "MyProject_VERSION_PATCH: ${MyProject_VERSION_PATCH}")
message(STATUS "MyProject_VERSION_TWEAK: ${MyProject_VERSION_TWEAK}")
message(STATUS "MyProject_DESCRIPTION: ${MyProject_DESCRIPTION}")
message(STATUS "MyProject_HOMEPAGE_URL: ${MyProject_HOMEPAGE_URL}")
```

```cmake
# common-project-include.cmake

include( "${CMAKE_CURRENT_SOURCE_DIR}/project-meta-info.cmake" )
```

```cmake
# project-meta-info.cmake

set( project_version 1.2.3.4 )
set( project_description "Description of root-project" )
set( project_homepage "https://www.root-project.com" )
```
