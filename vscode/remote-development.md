# Remote Development on Visual Studio Code

2019년 5월쯤 Microsoft에서 vscode를 이용한 원격 개발이 가능하도록 기능을 제공했다고 한다. `Remote Development` 라는 extension을 설치하면 된다. 크게 3가지 타입이 지원되는데, 일반 Linux PC, Docker container, 그리고 WSL(Windows Subsystem for Linux)이 있다. 한 가지 아쉬운 점은 연결 대상이 아직은 Linux 한정이라는 점이 있다. 본 기능은 아래 첫번째 영상을 통해 알게 되었고 두번째 링크를 통해 쉽게 원격개발환경을 설정할 수 있었다. 참고하기 바란다.

- [C++Now 2019, C++ Development with Visual Studio Code](https://youtu.be/knghWKWQmxg)
- [원격서버 vscode로 연결해서 작업하기](https://evols-atirev.tistory.com/28)
- [Remote Development using SSH](https://code.visualstudio.com/docs/remote/ssh)

## More Information

참고로 Visual Studio 2019에도 동일하게 Remote Development 기능이 추가되었다고 하니 필요하면 확인해 볼 수 있겠다.

## Remote Containers

Local PC에 설치된 Docker Container에 접속하는 방법은 다음과 같다.

- `F1` 또는 `Ctrl + Shift + P`를 입력하여 Command Pallete 로 진입한다.
- **_Remote-Containers: Attach to Running Container..._** 를 선택한다.
- 현재 running 중인 Container 중에서 원하는 것을 선택하면 접속이 완료된다.
