# iperf

네트워크 성능을 측정하는 도구로, throughput test를 할 때 이용한다. 여러 가지 옵션이 있으나 유용하게 사용했던 옵션만을 기록하였다.

```txt
-s, --server : run in server mode
-c, --client <host> : run in client mode, connecting to <host>
-f, --format : [kmKM] format to report: Kbits, Mbits, KBytes, MBytes
-t, --time n : time in seconds to transmit for (default 10 secs)
-P, --parallel n : number of parallel client threads to run
```

## Usage

server mode로 동작시킬 때는 아래와 같이 명령을 입력한다.

```sh
# KB 단위로 정보를 출력
> iperf -s -f K
```

client mode로 동작시킬 때는 아래와 같이 명령을 입력한다.

```sh
# 동시에 5개 세션으로 측정
> iperf -c 192.168.0.100 -P 5
```
