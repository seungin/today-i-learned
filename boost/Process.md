# Boost.Process

Boost.Process 라이브러리에 대해 설명한다. Boost.Process는 시스템에서 동작하는 프로세스를 관리하기 위한 라이브러리로 다음과 같은 일을 할 수 있다.

- 자식 프로세스 생성
- 자식 프로세스의 in/out stream 설정
- stream을 이용하여 자식 프로세스와 통신
- 자식 프로세스들이 종료될 때까지 대기
- 자식 프로세스를 종료

Boost.Process는 관례적으로 namespace를 아래와 같이 bp로 사용한다.

```cpp
namespace bp = boost::process;
```

### Starting a process

자식 프로세스를 생성하는 방법이 2가지가 있다. exe-args style로 실행할 때는 실행할 프로그램을 환경변수 PATH에서 찾기 위해 `search_path` 함수를 이용할 수 있다. 여기에서 사용하는 `system` 함수는 우리 프로그램이 자식 프로세스가 끝날 때까지 항상 기다린다.

```cpp
// command-line style
bp::system("g++ --version");

// exe-args style
bp::system("/usr/bin/g++", "--version");

// exe-args style (searching from $PATH)
boost::filesystem::path path = bp::search_path("g++");
bp::system(path, "--version");
```

### Launching a process

자식 프로세스를 async로 실행시키는 방법이 몇 가지 있다. system 함수의 비동기 버전인 async_system, spawn 함수와 child 클래스가 있다. `spawn` 함수는 자식 프로세스를 생성하고 바로 detach시킨다. 반면 `child` 클래스는 생성한 자식 프로세스를 기다리거나 종료하는 등 관리할 수 있다.

```cpp
#include <iostream>
#include <thread>

boost::filesystem::path path = bp::search_path("notepad");
bp::child child(path, "main.cpp");

while (child.running())
{
    std::cout << "child process is running..." << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

child.wait();
int result = child.exit_code();
std::cout << "exit_code: " << result << std::endl;
```

### Environment

프로그램 이름만으로 프로세스를 실행하기 위해서는 환경변수 PATH를 수정해야 할 수 있다. 아래와 같이 bp::search_path에 파라미터를 추가하면 특정 경로에 있는 프로그램을 실행할 수 있다.

```cpp
auto custom_path = boost::this_process::path();
custom_path.push_back("C:\\Program Files\\Notepad++");

std::string executable_name = "notepad++";
auto path = bp::search_path(executable_name, custom_path);

if (path.empty())
{
    std::cerr << executable_name << " is not found." << std::endl;
    return 0;
}

bp::child child(path, "main.cpp");
...
```
