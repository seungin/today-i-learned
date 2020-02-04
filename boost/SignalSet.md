# Boost.Asio.SignalSet

Boost.Asio 라이브러리의 SignalSet에 대해 설명한다. SignalSet은 크로스플랫폼 Signal 처리를 위한 기능을 제공한다.

### Register a signal handler asynchronously

비동기적으로 Signal Handler를 등록하고 io_context를 통해 signal 발생을 대기할 수 있다. 아래 코드는 Signal 발생 대기하는 것을 io_context에 맡기고 있다. `Ctrl + C` 를 입력하여 SIGINT를 발생시키면 Signal Handler가 호출된 후 io_context.run() 함수는 종료되어 빠져나와 이어서 메인 프로그램이 실행되게 된다.

```cpp
#include <atomic>
#include <iostream>
#include <thread>

#include <boost/asio/signal_set.hpp>

std::atomic<bool> isRunning = true;

void SignalHandler(const boost::system::error_code& error, int signal_number)
{
    isRunning = false;
}

int main()
{
    std::cout << "MainThread Start!" << std::endl;

    boost::asio::io_context io_context;
    boost::asio::signal_set signal_set(io_context);

    signal_set.add(SIGINT);
    signal_set.async_wait(&SignalHandler);

    io_context.run();

    std::cout << "MainThread End." << std::endl;
    return 0;
}
```
