# Getting timestamp using std::chrono

C++11의 std::chrono 라이브러리를 이용하면 쉽게 timestamp를 얻을 수 있다.  
먼저 개념을 잘 이해해야 할 것이 time_point와 duration이다.  
time_point는 특정 시각을 의미하며 duration은 시각과 시각 사이 간격이다.  
이 개념을 알고 본다면 그리 어렵지 않을 것이다.

[Getting timestamp using std::chrono]

## Duration

duration은 단위 시간에 따른 변환이 쉽게 가능하다.  
아래 예제와 같이 똑같은 1시간을 단위에 따라 1, 3600, 3600000 등으로 표현한다.  

```cpp
std::string time_duration() {
    auto duration_hour = std::chrono::hours(1);
    auto duration_sec = std::chrono::duration_cast<std::chrono::seconds>(duration_hour);
    auto duration_ms = std::chrono::duration_cast<std::chrono::milliseconds>(duration_hour);
    auto duration_us = std::chrono::duration_cast<std::chrono::microseconds>(duration_hour);

    std::string string;
    string += std::to_string(duration_hour.count()) + " hour = ";
    string += std::to_string(duration_sec.count()) + " seconds = ";
    string += std::to_string(duration_ms.count()) + " milliseconds = ";
    string += std::to_string(duration_us.count()) + " microseconds";
    return string;
}
```

## Time Stamp

실제 timestamp는 어떻게 얻을 수 있을까?  
기준 시각(epoch)으로부터 특정 시각까지의 duration으로 표현한다.  
기본적으로 nanoseconds 단위를 사용하여 int64_t 변수에 담는다.  

```cpp
int64_t time_stamp() {
    std::chrono::system_clock::time_point time_point = std::chrono::system_clock::now();
    std::chrono::system_clock::duration duration = time_point.time_since_epoch();
    int64_t timestamp = duration.count();
    return timestamp;
}
```

## Formatted Time String

timestamp는 그 자체로는 의미하는 바를 알아보기 어렵다.  
기존의 C/C++ Library에서 제공하는 기능을 이용하여 사람이 읽기 쉬운 문자열로 변환할 수 있다.  

```cpp
std::string time_string() {
    std::chrono::system_clock::time_point time_point = std::chrono::system_clock::now();
    std::time_t time_t = std::chrono::system_clock::to_time_t(time_point);
    std::tm* tm = std::localtime(&time_t);

    std::stringstream stream;
    stream << std::put_time(tm, "%Y-%m-%d %X");
    return stream.str();
}
```

## Formatted Time Stamp

timestamp를 log에 이용할 때는 위와 같은 formatted time string을 이용하면 좋다.  
거기에 실시간 resolution을 나타내기 위해 microseconds 단위까지 표시해보자.  
default duration의 단위가 nanoseconds 단위이므로 microseconds로 단위 변환하면 된다.  

```cpp
std::string time_stamp_formatted() {
    std::string date_time = time_string();
    std::string microseconds = std::to_string((time_stamp() / 1000) % 1000000);
    return date_time + "." + microseconds;
}
```

## Result

main 함수를 실행하면 아래와 같은 결과를 볼 수 있다.

```cpp
int main() {
    std::cout << time_duration() << '\n';
    std::cout << time_stamp() << '\n';
    std::cout << time_string() << '\n';
    std::cout << time_stamp_formatted() << '\n';
    return 0;
}
```

> 1 hour = 3600 seconds = 3600000 milliseconds = 3600000000 microseconds
> 1516373058784794000
> 2018-01-19 23:44:18
> 2018-01-19 23:44:18.786704

[Getting timestamp using std::chrono]:https://github.com/seungin/cpp/blob/master/getting_timestamp_using_chrono/main.cpp
