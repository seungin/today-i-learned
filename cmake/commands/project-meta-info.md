# usecase : project-meta-info

프로젝트 버전, 프로젝트에 대한 간단한 설명, 프로젝트 홈페이지 등의 메타 데이터를 외부 파일에서 정의하여 사용하는 방법을 설명한다. 외부 파일을 사용하면 Project의 CMakeLists.txt 파일을 수정하지 않아도 되는 장점이 있다.

## Examples

```cmake
# CMakeLists.txt

cmake_minimum_required( VERSION 3.15...3.17 )

include( "${CMAKE_CURRENT_SOURCE_DIR}/project-meta-info.cmake" )

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
# project-meta-info.cmake

set( project_version 1.2.3.4 )
set( project_description "Description of root-project" )
set( project_homepage "https://www.root-project.com" )
```
