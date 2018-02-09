# Memory leak debugging with visual studio

Visual C++을 사용하면 Memory Leak 탐지를 위해 `CRTDBG` 기능을 이용할 수 있다.  
이 기능은 DEBUG 실행할 때에만 적용됨에 주의하며 아래 예제 코드를 보자.  

[Memory leak debugging with visual studio]

## Enable CRTDBG

[Official Documentation]에서는 아래와 같은 순서대로 정의해야 CRT 함수가 제대로 작동한다고 말하고 있다.  

```cpp
// Show file name and line number of the object allocated by malloc()
#define _CRTDBG_MAP_ALLOC
#include <stdlib.h>
#include <crtdbg.h>
```

## Report always memory leaks when the program exits

CRTDBG 기능을 이용하면 원하는 위치에서 memory leak을 탐지할 수도 있지만  
보통은 leak이 발생하면 프로그램 종료 시에 항상 뜨도록 사용하는 편이다.  
이를 위해서 몇 가지 Flag를 설정하여 `_CrtSetDbgFlag() 함수`를 호출해주면 된다.  

```cpp
#define CHECK_MEMORY_LEAK() _CrtSetDbgFlag(_CRTDBG_ALLOC_MEM_DF | _CRTDBG_LEAK_CHECK_DF)
```

## Report targeting: not only malloc, but also new

`_CRTDBG_MAP_ALLOC`을 정의(define)하지 않은 경우 detecting 정보가 모호할 수 있다.  

```txt
Detected memory leaks!
Dumping objects -> {18} normal block at 0x00780E80
```

`_CRTDBG_MAP_ALLOC`을 정의하면 보고서에 파일 이름, 라인 번호 등 자세한 내용이 표시된다.  

```txt
Detected memory leaks!
Dumping objects -> main.cpp(7) : {18} normal block at 0x00780E80
```

하지만 new 연산자에 대해서는 이러한 정보가 출력되지 않으므로 new를 재정의해야 한다.  
아래 코드와 같이 재정의한 후, new 대신 DBG_NEW 를 사용하면 된다.  

```cpp
#ifndef DBG_NEW
#define DBG_NEW new(_NORMAL_BLOCK, __FILE__, __LINE__)
#endif
```

## Result

main 함수를 실행하면 아래와 같은 결과를 볼 수 있다.

```cpp
#include "debug.h"

int main() {
  CHECK_MEMORY_LEAK();

  int* valuePtr;
  valuePtr = (int*)malloc(sizeof(int));
  valuePtr = DBG_NEW int;
  return 0;
}
```

> Dumping objects ->  
> main.cpp(8) : {81} normal block at 0x03236110, 4 bytes long.  
>  Data: <    > CD CD CD CD  
> main.cpp(7) : {80} normal block at 0x032360E0, 4 bytes long.  
>  Data: <    > CD CD CD CD  
> Object dump complete.  

[Memory leak debugging with visual studio]:https://github.com/seungin/msvc/blob/master/memory-leak-debugging-with-visual-studio/memory-leak-debugging-with-visual-studio/debug.h
[Official Documentation]:https://msdn.microsoft.com/ko-kr/library/x98tx3cf.aspx?f=255&MSPPError=-2147217396