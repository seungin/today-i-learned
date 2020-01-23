# Using Mingw-w64 in VS Code

본 내용은 Windows 환경에서 VS Code와 Mingw-w64 컴파일러를 이용한 C/C++ 개발 환경 구축에 대해 설명한다. 아래 링크의 내용을 기반으로 작성되었으므로 상세한 내용은 링크를 참고하면 된다.

[Using Mingw-w64 in VS Code](https://code.visualstudio.com/docs/cpp/config-mingw)

## Prerequisites

1. [Visual Studio Code](https://code.visualstudio.com/download)를 설치한다.
2. VS Code에서 [C/C++ Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)을 설치한다.
3. [Mingw-w64](http://mingw-w64.org/doku.php/download/mingw-builds)를 설치한다.

    - Mingw-w64 설치가 완료되면 툴체인 바이너리 경로를 PATH에 추가한다.
    - 기본 경로인 C:\mingw-w64\x86_64-8.1.0-win32-seh-rt_v6-rev0\mingw64\bin 와 비슷할 것이다.
    - 확인을 위해 아래 명령이 정상적으로 수행하는지 확인한다.

        ```bat
        > g++ --version
        g++ (i686-posix-dwarf-rev0, Built by MinGW-W64 project) 8.1.0
        Copyright (C) 2018 Free Software Foundation, Inc.
        This is free software; see the source for copying conditions.  There is NO
        warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

        > gdb --version
        GNU gdb (GDB) 8.1
        Copyright (C) 2018 Free Software Foundation, Inc.
        License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
        This is free software: you are free to change and redistribute it.
        There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
        and "show warranty" for details.
        This GDB was configured as "i686-w64-mingw32".
        Type "show configuration" for configuration details.
        For bug reporting instructions, please see:
        <http://www.gnu.org/software/gdb/bugs/>.
        Find the GDB manual and other documentation resources online at:
        <http://www.gnu.org/software/gdb/documentation/>.
        For help, type "help".
        Type "apropos word" to search for commands related to "word".
        ```

## Create a Simple C++ App

VS Code를 이용하여 C++ 개발을 하는데 필요한 3가지 파일이 있다.

- tasks.json
- launch.json
- c_cpp_properties.json

### `tasks.json` Build main.cpp

- **_Terminal > Configure Default Build Task_** 를 선택한다.
- **_C/C++: g++.exe build active file_** 을 선택한다.
- 그러면 아래와 같은 파일이 자동 생성된다.
- 참고로, tasks.json 파일에서 사용하는 변수들을 알고 싶으면 [variables reference](https://code.visualstudio.com/docs/editor/variables-reference) 를 참고한다.

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "type": "shell",
      "label": "g++.exe build active file",
      "command": "C:\\mingw-w64\\i686-8.1.0-posix-dwarf-rt_v6-rev0\\mingw32\\bin\\g++.exe",
      "args": ["-g", "${file}", "-o", "${fileDirname}\\${fileBasenameNoExtension}.exe"],
      "options": {
        "cwd": "C:\\mingw-w64\\i686-8.1.0-posix-dwarf-rt_v6-rev0\\mingw32\\bin"
      },
      "problemMatcher": ["$gcc"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    }
  ]
}
```

- 한번 파일을 생성해두면 다음부터는 `Ctrl + Shift + B` 단축키를 이용할 수 있다.
- 빌드가 완료되면 Terminal을 하나 더 열어서 실행해볼 수 있다.

```bat
PS C:\Users\seungin\Workspace\VisualStudioCode> .\main.exe
Hello C++ World from VS Code and the C++ extension!
```

### `launch.json` Debug main.cpp

- **_Debug > Add Configuration_** 을 선택한다.
- **_C++ (GDB/LLDB)_** 를 선택한다.
- 그러면 아래와 같은 파일이 자동 생성된다.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++.exe build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\mingw-w64\\i686-8.1.0-posix-dwarf-rt_v6-rev0\\mingw32\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "g++.exe build active file"
    }
  ]
}
```

- main.cpp 파일에 원하는 위치에 break point를 걸어둔다. 단축키 `F9`를 이용할 수 있다.
- **_Debug > Start Debugging_** 을 선택하여 디버깅을 시작한다. 단축키 `F5`를 이용할 수 있다.
- `Ctrl + Shift + D` 단축키를 이용하여 Debug View로 이동하여 디버깅을 진행할 수 있다.

### `c_cpp_properties.json` Change settings such as compiler, include paths, C++ standard, ...

- **_C/C++: Edit Configurations (UI)_** 을 선택한다.
- `.vscode\c_cpp_properties.json` 파일을 직접 수정할 수도 있다.
- 생성된 파일 내용은 아래와 같다.

```json
{
  "configurations": [
    {
      "name": "Win32",
      "includePath": ["${workspaceFolder}/**"],
      "defines": ["_DEBUG", "UNICODE", "_UNICODE"],
      "compilerPath": "C:\\mingw-w64\\i686-8.1.0-posix-dwarf-rt_v6-rev0\\mingw32\\bin\\gcc.exe",
      "cStandard": "c11",
      "cppStandard": "c++17",
      "intelliSenseMode": "clang-x86"
    }
  ],
  "version": 4
}
```
