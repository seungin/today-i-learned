# Windows에서 사용 중인 Port 확인하기

특정 Port가 현재 사용 중인지, 사용 중인 프로세스는 누구인지 확인하는 방법을 설명한다.

## Using netstat command

Jenkins 기본 Port인 8080을 이미 사용 중인지 netstat 명령을 이용하여 확인할 수 있다.

```sh
$ netstat -ano | grep 8080
  Proto  Local Address          Foreign Address        State           PID
  TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING       17656
  TCP    [::]:8080              [::]:0                 LISTENING       17656
  TCP    [::1]:8080             [::1]:59689            ESTABLISHED     17656
  TCP    [::1]:59689            [::1]:8080             ESTABLISHED     1856
```

PID 17656에 해당하는 프로세스를 확인하려면 작업관리자를 연다. 단축키를 이용하려면 `Ctrl + Shift + Esc`를 누른다. 세부 정보 창에 가서 해당 PID에 해당하는 프로세스를 확인한다.
