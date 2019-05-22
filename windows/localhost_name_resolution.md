# "127.0.0.1" 대신에 "localhost" 라는 이름을 사용할 때 주의점

Windows에서 boost::asio client socket을 사용할 때 endpoint를 생성하기 위해 ip address와 port를 사용한다.  
이 때 resolver를 이용하면 domain name으로부터 ip address를 구할 수도 있다.  

이 방법을 이용하면 loopback interface을 사용할 때 아래와 같이 2가지 방법이 있다.

- "127.0.0.1"
- "localhost"

Linux의 경우에는 두 가지 모두 성능 상 차이가 없는 것으로 보였는데  
Windows에서는 "localhost"라는 이름을 이용하면 1초 정도 delay가 발생하였다.  

이것은 DNS를 통한 name resolution에 따른 지연 시간이 발생한 것인데,  
개발 중 지연 시간을 제거하기 위해서는 127.0.0.1과 같이 ip address를 그대로 입력해서 사용해야 한다.  

추가적으로 Windows에서는 hosts 파일을 통해 마치 DNS처럼 mapping된 ip address를 바로 이용하는 방법이 있다.  
이 파일에 정의되지 않았거나 아래와 같이 주석 처리가 되어 있으면 실제 DNS를 통해 name resolution을 수행하게 된다.  

`C:\Windows\System32\drivers\etc\hosts`

    # Copyright (c) 1993-2009 Microsoft Corp.
    #
    # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
    #
    # This file contains the mappings of IP addresses to host names. Each
    # entry should be kept on an individual line. The IP address should
    # be placed in the first column followed by the corresponding host name.
    # The IP address and the host name should be separated by at least one
    # space.
    #
    # Additionally, comments (such as these) may be inserted on individual
    # lines or following the machine name denoted by a '#' symbol.
    #
    # For example:
    #
    #      102.54.94.97     rhino.acme.com          # source server
    #       38.25.63.10     x.acme.com              # x client host
    
    # localhost name resolution is handled within DNS itself.
    #	127.0.0.1       localhost
    #	::1             localhost
